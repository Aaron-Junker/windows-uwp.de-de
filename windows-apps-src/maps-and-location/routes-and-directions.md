---
title: Anzeigen von Routen und Wegbeschreibungen auf einer Karte
description: Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie sie in Ihrer App an.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: Windows 10, UWP, Route, Karte, Standort, Wegbeschreibungen
ms.localizationpriority: medium
ms.openlocfilehash: 218ca052a3e525a1f7cfc2ce18542a5a30c61e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646685"
---
# <a name="display-routes-and-directions-on-a-map"></a>Anzeigen von Routen und Wegbeschreibungen auf einer Karte



Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie sie in Ihrer App an.

>[!Note]
>Laden Sie das [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619977) herunter, um mehr über die Verwendung von Karten in Ihrer App zu erfahren.
>Wenn die Kartenfunktion kein zentrales Feature Ihrer App ist, sollten Sie stattdessen die Windows-Karten-App starten. Sie können die URI-Schemas `bingmaps:`, `ms-drive-to:` und `ms-walk-to:` zum Starten der Windows-Karten-App für bestimmte Karten und für Wegbeschreibungen mit Sprachnavigation verwenden. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://msdn.microsoft.com/library/windows/apps/mt228341).

 
## <a name="an-intro-to-maproutefinder-results"></a>Eine Einführung in die MapRouteFinder-Ergebnisse


Hier erfahren Sie, wie Klassen für Routen und Wegbeschreibungen zusammenhängen.

* Die [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938)-Klasse verfügt über Methoden zum Abrufen von Routen und Wegbeschreibungen. Diese Methoden geben ein [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) zurück.

* Das [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) enthält ein [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937)-Objekt. Auf dieses Objekt greifen Sie über die [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940)-Eigenschaft des **MapRouteFinderResult** zu.

* Die [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) enthält eine Sammlung von [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955)-Objekten. Auf diese Sammlung greifen Sie über die [**Legs**](https://msdn.microsoft.com/library/windows/apps/dn636973)-Eigenschaft der **MapRoute** zu.

* Jeder [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) enthält eine Sammlung von [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961)-Objekten. Auf diese Sammlung greifen Sie über die [**Maneuvers**](https://msdn.microsoft.com/library/windows/apps/dn636959)-Eigenschaft des **MapRouteLeg** zu.

Rufen Sie Routen und Wegbeschreibungen für Auto und Fußgänger ab, indem Sie die Methoden der [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938)-Klasse aufrufen. Beispielsweise [**GetDrivingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636943) oder [**GetWalkingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636953).

Wenn Sie eine Route anfordern, können Sie Folgendes angeben:

* Sie können nur einen Startpunkt und einen Endpunkt oder eine Reihe von Wegpunkten zur Berechnung angeben.

    *Endwegpunkte* fügt zusätzliche Etappen jeweils mit eigener Reiseroute hinzu. Um *Endwegpunkte* anzugeben, verwenden Sie die [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync)-Überladungen.

    *Über* Wegpunkt werden Zwischenpositionen zwischen *Endwegpunkten* definiert. Es werden keine Teilstrecken der Route hinzugefügt.  Es handelt sich lediglich um Wegpunkte, die eine Route durchlaufen muss. Um *Über*-Wegpunkte festzulegen, verwenden Sie eine der [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync)-Überladungen.

* Sie können Optimierungen angeben (z. B. die Minimierung der Distanz).

* Sie können Einschränkungen festlegen (z. B. das Vermeiden von Autobahnen).

## <a name="display-directions"></a>Anzeigen von Wegbeschreibungen

Das [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939)-Objekt enthält ein [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937)-Objekt, auf das Sie über seine [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940)-Eigenschaft zugreifen können.

Die berechnete [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) hat Eigenschaften, die die Zeit zum Zurücklegen der Route, die Länge der Route und die Auflistung von [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955)-Objekten bereitstellen, die die Teilstrecken der Route enthalten. Jedes **MapRouteLeg**-Objekt enthält eine Auflistung von [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961)-Objekten. Das **MapRouteManeuver**-Objekt enthält eine Wegbeschreibung, auf die Sie über seine [**InstructionText**](https://msdn.microsoft.com/library/windows/apps/dn636964)-Eigenschaft zugreifen können.

>[!IMPORTANT]
>Sie müssen einen Kartenauthentifizierungsschlüssel angeben, bevor Sie Kartendienste verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

Dieses Beispiel zeigt die folgenden Ergebnisse im `tbOutputText`-Textfeld an:

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>Anzeigen von Routen


Blenden Sie eine [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) auf einem [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) ein, indem Sie eine [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) mit der **MapRoute** erstellen. Fügen Sie die **MapRouteView** anschließend zur [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047)-Auflistung von **MapControl** hinzu.

>[!IMPORTANT]
>Sie müssen einen Kartenauthentifizierungsschlüssel angeben, bevor Sie Kartendienste oder das Kartensteuerelement verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

Dieses Beispiel zeigt Folgendes in einem [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) namens **MapWithRoute** an.

![Kartensteuerelement mit angezeigter Route](images/routeonmap.png)

Nachfolgend finden Sie eine Version dieses Beispiels, das einen *Über*-Wegpunkt zwischen zwei *Endwegpunkten* verwendet:

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Entwurfsrichtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://go.microsoft.com/fwlink/p/?LinkId=619982)
