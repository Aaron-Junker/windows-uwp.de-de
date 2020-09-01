---
title: Anzeigen von Routen und Wegbeschreibungen auf einer Karte
description: Erfahren Sie, wie Sie Routen und Directions mithilfe der maproutefinder-Klasse abrufen und in einem mapcontrol-Element in einer universelle Windows-Plattform-app (UWP) anzeigen.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: Windows 10, UWP, Route, map, Location, Directions
ms.localizationpriority: medium
ms.openlocfilehash: b015393d81d736e5886793431966d0d91976e80d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171754"
---
# <a name="display-routes-and-directions-on-a-map"></a>Anzeigen von Routen und Wegbeschreibungen auf einer Karte



Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie sie in Ihrer App an.

>[!Note]
>Wenn Sie weitere Informationen zum Verwenden von Maps in Ihrer APP haben, laden Sie das [Map-Beispiel für universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)herunter.
>Wenn die Zuordnung kein zentrales Feature Ihrer APP ist, sollten Sie stattdessen die Windows Maps-app starten. Sie können die URI-Schemas `bingmaps:`, `ms-drive-to:` und `ms-walk-to:` zum Starten der Windows-Karten-App für bestimmte Karten und für Wegbeschreibungen mit Sprachnavigation verwenden. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](../launch-resume/launch-maps-app.md).

 
## <a name="an-intro-to-maproutefinder-results"></a>Eine Einführung in die MapRouteFinder-Ergebnisse


Hier erfahren Sie, wie Klassen für Routen und Wegbeschreibungen zusammenhängen.

* Die [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder)-Klasse verfügt über Methoden zum Abrufen von Routen und Wegbeschreibungen. Diese Methoden geben ein [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult) zurück.

* Das [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult) enthält ein [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute)-Objekt. Greifen Sie über die [**Route**](/uwp/api/windows.services.maps.maproutefinderresult.route) -Eigenschaft von **maproutefinderresult**auf dieses Objekt zu.

* Die [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) enthält eine Sammlung von [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg)-Objekten. Greifen Sie über die Eigenschaft [**Legs**](/uwp/api/windows.services.maps.maproute.legs) der **MapRoute**auf diese Auflistung zu.

* Jeder [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg) enthält eine Sammlung von [**MapRouteManeuver**](/uwp/api/Windows.Services.Maps.MapRouteManeuver)-Objekten. Greifen Sie über die Eigenschaft " [**Manöver**](/uwp/api/windows.services.maps.maprouteleg.maneuvers) " von **maprouteleg**auf diese Auflistung zu.

Rufen Sie mithilfe der Methoden der [**maproutefinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) -Klasse eine Driving-oder Walking-Route und Anweisungen ab. Beispiel: [**getdrivingrouteasync**](/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) oder [**getwalkingrouteasync**](/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync).

Wenn Sie eine Route anfordern, können Sie Folgendes angeben:

* Sie können nur einen Startpunkt und einen Endpunkt oder eine Reihe von Wegpunkten zur Berechnung angeben.

    Mit der *Beendigung* von "Wegpunkte" werden zusätzliche Routen mit jeweils eigenem Routen hinzugefügt. Verwenden Sie eine der [**getdrivingroutefromwaypoinzasync**](/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync) -über Ladungen, um die Beendens von *Endpunkten* anzugeben.

    *Über* "Wegpunkt" definiert Zwischenspeicher Orte zwischen Haltepunkten für das *Ende* . Sie fügen keine Routen Beine hinzu.  Dabei handelt es sich lediglich um Punkte, die eine Route durchlaufen muss. Verwenden Sie eine der [**getdrivingroutefromenhancedwaypoinstiasync**](/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync) -über Ladungen, um Sie *über* "Waypoints" anzugeben.

* Sie können Optimierungen angeben (z. b. den Abstand minimieren).

* Sie können Einschränkungen angeben (z. b. vermeiden von Autobahnen).

## <a name="display-directions"></a>Anzeigen von Wegbeschreibungen

Das [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult)-Objekt enthält ein [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute)-Objekt, auf das Sie über seine [**Route**](/uwp/api/windows.services.maps.maproutefinderresult.route)-Eigenschaft zugreifen können.

Die berechnete [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) hat Eigenschaften, die die Zeit zum Zurücklegen der Route, die Länge der Route und die Auflistung von [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg)-Objekten bereitstellen, die die Teilstrecken der Route enthalten. Jedes **MapRouteLeg**-Objekt enthält eine Auflistung von [**MapRouteManeuver**](/uwp/api/Windows.Services.Maps.MapRouteManeuver)-Objekten. Das **MapRouteManeuver**-Objekt enthält eine Wegbeschreibung, auf die Sie über seine [**InstructionText**](/uwp/api/windows.services.maps.maproutemaneuver.instructiontext)-Eigenschaft zugreifen können.

>[!IMPORTANT]
>Sie müssen einen Maps-Authentifizierungsschlüssel angeben, bevor Sie map-Dienste verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

 

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


Blenden Sie eine [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) auf einem [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) ein, indem Sie eine [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) mit der **** MapRoute erstellen. Fügen Sie dann **maprouteview** der [**Routes**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) -Auflistung des **mapcontrol-Steuer**Elements hinzu.

>[!IMPORTANT]
>Sie müssen einen Maps-Authentifizierungsschlüssel angeben, bevor Sie Kartendienste oder das Karten Steuerelement verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

 

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

In diesem Beispiel werden die folgenden Elemente in einem [**mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) namens **mapwithroute**angezeigt.

![Kartensteuerelement mit angezeigter Route](images/routeonmap.png)

Im folgenden finden Sie eine Version dieses Beispiels, bei der ein *via* -Wegpunkt zwischen zwei *Endpunkten* verwendet wird:

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

## <a name="related-topics"></a>Zugehörige Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Entwurfsrichtlinien für Karten](./display-maps.md)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)