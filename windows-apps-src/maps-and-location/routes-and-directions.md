---
title: Anzeigen von Routen und Wegbeschreibungen auf einer Karte
description: Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie diese in Ihrer App an.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: Windows 10, UWP, Route, Karte, Standort, Wegbeschreibungen
ms.localizationpriority: medium
ms.openlocfilehash: e9e464f9a3b49d3a94edbc8593df58e1e7c24515
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259336"
---
# <a name="display-routes-and-directions-on-a-map"></a>Anzeigen von Routen und Wegbeschreibungen auf einer Karte



Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie diese in Ihrer App an.

>[!Note]
>Laden Sie das [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) herunter, um mehr über die Verwendung von Karten in Ihrer App zu erfahren.
>Wenn die Kartenfunktion kein zentrales Feature Ihrer App ist, sollten Sie stattdessen die Windows-Karten-App starten. Sie können die URI-Schemas `bingmaps:`, `ms-drive-to:` und `ms-walk-to:` zum Starten der Windows-Karten-App für bestimmte Karten und für Turn-by-Turn-Wegbeschreibungen verwenden. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app).

 
## <a name="an-intro-to-maproutefinder-results"></a>Eine Einführung in die MapRouteFinder-Ergebnisse


Hier erfahren Sie, wie Klassen für Routen und Wegbeschreibungen zusammenhängen.

* Die [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder)-Klasse verfügt über Methoden zum Abrufen von Routen und Wegbeschreibungen. Diese Methoden geben ein [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) zurück.

* Das [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) enthält ein [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute)-Objekt. Auf dieses Objekt greifen Sie über die [**Route**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route)-Eigenschaft des **MapRouteFinderResult** zu.

* Die [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) enthält eine Sammlung von [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg)-Objekten. Auf diese Sammlung greifen Sie über die [**Legs**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute.legs)-Eigenschaft der **MapRoute** zu.

* Jeder [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) enthält eine Sammlung von [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver)-Objekten. Auf diese Sammlung greifen Sie über die [**Maneuvers**](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteleg.maneuvers)-Eigenschaft des **MapRouteLeg** zu.

Rufen Sie Routen und Wegbeschreibungen für Auto und Fußgänger ab, indem Sie die Methoden der [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder)-Klasse aufrufen. Beispielsweise [**GetDrivingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) oder [**GetWalkingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync).

Wenn Sie eine Route anfordern, können Sie Folgendes angeben:

* Sie können nur einen Startpunkt und einen Endpunkt oder eine Reihe von Wegpunkten zur Berechnung angeben.

    *Endwegpunkte* fügt zusätzliche Etappen jeweils mit eigener Reiseroute hinzu. Um *Endwegpunkte* anzugeben, verwenden Sie die [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync)-Überladungen.

    *Über* Wegpunkt werden Zwischenpositionen zwischen *Endwegpunkten* definiert. Es werden keine Teilstrecken der Route hinzugefügt.  Es handelt sich lediglich um Wegpunkte, die eine Route durchlaufen muss. Um *Über*-Wegpunkte festzulegen, verwenden Sie eine der [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync)-Überladungen.

* Sie können Optimierungen angeben (z. B. die Minimierung der Distanz).

* Sie können Einschränkungen festlegen (z. B. das Vermeiden von Autobahnen).

## <a name="display-directions"></a>Anzeigen von Wegbeschreibungen

Das [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult)-Objekt enthält ein [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute)-Objekt, auf das Sie über seine [**Route**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route)-Eigenschaft zugreifen können.

Die berechnete [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) hat Eigenschaften, die die Zeit zum Zurücklegen der Route, die Länge der Route und die Auflistung von [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg)-Objekten bereitstellen, die die Teilstrecken der Route enthalten. Jedes **MapRouteLeg**-Objekt enthält eine Auflistung von [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver)-Objekten. Das **MapRouteManeuver**-Objekt enthält eine Wegbeschreibung, auf die Sie über seine [**InstructionText**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver.instructiontext)-Eigenschaft zugreifen können.

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


Blenden Sie eine [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) auf einem [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) ein, indem Sie eine [**MapRouteView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) mit derMapRoute erstellen. Fügen Sie die **MapRouteView** anschließend zur [**Routes**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes)-Auflistung von **MapControl** hinzu.

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

Dieses Beispiel zeigt Folgendes in einem [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) namens **MapWithRoute** an.

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
* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Entwurfsrichtlinien für Karten](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Build 2015 video: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)
