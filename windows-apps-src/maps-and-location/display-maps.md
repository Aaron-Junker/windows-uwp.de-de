---
title: Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten
description: Sie können eine Zuordnung in einem hell verbrechbaren Fenster anzeigen, das als Karte Karte *platzieren* bezeichnet wird, oder in einem Karten Steuerelement mit vollem Funktionsumfang.
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, map, Location, Map-Steuerelement, Kartenansichten
ms.localizationpriority: medium
ms.openlocfilehash: 1979aa2b2e99a585a122e4835bb6920b9d342cd7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162644"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten

Sie können eine Karte in einem hell verbrechbaren Fenster mit dem Namen "Map *placecard* " oder einem Karten Steuerelement mit vollem Funktionsumfang anzeigen.

Laden Sie das [Map-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) herunter, um einige der in diesem Handbuch beschriebenen Features auszuprobieren.

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>Karte in einer Platz Card anzeigen
Sie können Benutzern eine Karte in einem einfachen Popup Fenster oberhalb, unterhalb oder auf der Seite eines UI-Elements oder eines Bereichs einer App anzeigen, in dem der Benutzer berührt. In der Karte kann eine Stadt oder eine Adresse angezeigt werden, die sich auf Informationen in Ihrer APP bezieht.  

Diese Platzkarte zeigt die Stadt Seattle an.

![Platzhalter der Stadt Seattle](images/placecard-city.png)

Hier ist der Code, mit dem Seattle in einer Platzkarte unterhalb einer Schaltfläche angezeigt wird.

```csharp
private void Seattle_Click(object sender, RoutedEventArgs e)
{
    Geopoint seattlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6062, Longitude = -122.3321 });

    PlaceInfo spaceNeedlePlace = PlaceInfo.Create(seattlePoint);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

Diese Platzkarte zeigt den Speicherort der Leerraum Nadel in Seattle an.

![Platzhalter zeigt den Speicherort der Leerraum Nadel an](images/placecard-needle.png)

Hier ist der Code, mit dem die Leerraum-Nadel in einer Platzkarte unterhalb einer Schaltfläche angezeigt wird.

```csharp
private void SpaceNeedle_Click(object sender, RoutedEventArgs e)
{
    Geopoint spaceNeedlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6205, Longitude = -122.3493 });

    PlaceInfoCreateOptions options = new PlaceInfoCreateOptions();

    options.DisplayAddress = "400 Broad St, Seattle, WA 98109";
    options.DisplayName = "Seattle Space Needle";

    PlaceInfo spaceNeedlePlace =  PlaceInfo.Create(spaceNeedlePoint, options);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

<a id="map-control" />

## <a name="display-map-in-a-control"></a>Anzeigen einer Zuordnung in einem Steuerelement

Verwenden Sie ein Karten Steuerelement, um umfangreiche und anpassbare Kartendaten in der APP anzuzeigen. Ein Karten Steuerelement kann Road Maps, Luftbild, 3D, Ansichten, Richtungen, Suchergebnisse und Datenverkehr anzeigen. In einer Karte können Sie die Position des Benutzers, Wegbeschreibungen und interessante Orte anzeigen. Zudem kann eine Karte 3D-Luftbilder, Streetside-Ansichten, den Verkehr, öffentliche Verkehrsmittel und lokale Unternehmen enthalten.

Verwenden Sie ein Kartensteuerelement, wenn Sie in Ihrer App eine Karte benötigen, auf der Benutzer App-spezifische oder allgemeine geografische Informationen anzeigen können. Wenn die App ein Kartensteuerelement enthält, müssen Benutzer die App für diese Informationen nicht verlassen.

> [!NOTE]
>Wenn Sie sich keine Gedanken machen, dass Benutzer außerhalb Ihrer APP arbeiten, sollten Sie die Windows Maps-APP verwenden, um diese Informationen bereitzustellen. Ihre App kann die Windows-Karten-App starten, um bestimmte Karten, Wegbeschreibungen und Suchergebnisse anzuzeigen. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](../launch-resume/launch-maps-app.md).

### <a name="add-a-map-control-to-your-app"></a>Hinzufügen eines Karten Steuer Elements zu Ihrer APP

Zeigen Sie eine Karte auf einer XAML-Seite durch Hinzufügen eines [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) an. Um **MapControl** zu verwenden, müssen Sie den Namespace [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) in der XAML-Seite oder im Code deklarieren. Wenn Sie das Steuerelement der Toolbox entnehmen, wird die Namespacedeklaration automatisch hinzugefügt. Wenn Sie **MapControl** manuell zur XAML-Seite hinzufügen, müssen Sie die Namespacedeklaration oben auf der Seite manuell hinzufügen.

Das folgende Beispiel zeigt ein einfaches Kartensteuerelement. Außerdem wird die Karte so konfiguriert, dass Toucheingaben möglich sind und gleichzeitig die Steuerelemente für Zoom und Neigung angezeigt werden.

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>

 </Grid>
</Page>
```

Wenn Sie das Kartensteuerelement im Code hinzufügen, müssen Sie den Namespace oben in der Codedatei manuell deklarieren.

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

### <a name="get-and-set-a-maps-authentication-key"></a>Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels

Bevor Sie [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) und Kartendienste verwenden können, müssen Sie den Kartenauthentifizierungsschlüssel als Wert für die [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)-Eigenschaft angeben. Ersetzen Sie in den vorherigen Beispielen `EnterYourAuthenticationKeyHere` durch den Schlüssel, den Sie über das [Bing Maps Developer Center](https://www.bingmapsportal.com/) abgerufen haben. Bis Sie den Kartenauthentifizierungsschlüssel angeben, wird unterhalb des Steuerelements weiterhin der Text **Warnung: MapServiceToken wurde nicht angegeben** angezeigt. Weitere Informationen zum Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

## <a name="set-the-location-of-a-map"></a>Festlegen des Speicher Orts einer Karte
Zeigen Sie die Zuordnung auf eine beliebige Stelle, die Sie verwenden möchten, oder verwenden Sie den aktuellen Speicherort des Benutzers.  

### <a name="set-a-starting-location-for-the-map"></a>Festlegen einer Ausgangsposition für die Karte

Legen Sie die Position fest, die auf der Karte angezeigt werden soll, indem Sie die [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center)-Eigenschaft von [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) im Code angeben oder die Eigenschaft im XAML-Markup binden. Im folgenden Beispiel wird eine Karte mit Seattle in der Mitte angezeigt.

> [!NOTE]
> Da eine Zeichenfolge nicht in ein [**geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)konvertiert werden kann, können Sie keinen Wert für die [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) -Eigenschaft in XAML-Markup angeben, es sei denn, Sie verwenden die Datenbindung. (Diese Einschränkung gilt auch für die angefügte Eigenschaft [**MapControl.Location**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setlocation).

 
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![Beispiel für das Kartensteuerelement](images/displaymapsexample1.png)

### <a name="set-the-current-location-of-the-map"></a>Festlegen der aktuellen Kartenposition

Bevor die App auf die Position des Benutzers zugreifen kann, muss die App die [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)-Methode aufrufen. Zu diesem Zeitpunkt muss sich Ihre App im Vordergrund befinden, und **RequestAccessAsync** muss vom UI-Thread aufgerufen werden. Solange der Benutzer Ihrer App keinen Zugriff auf seine Position gewährt hat, kann Ihre App nicht auf Positionsdaten zugreifen.

Rufen Sie mit der [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)-Methode der Klasse [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) die aktuelle Position des Geräts ab (wenn die Position verfügbar ist). Zum Abrufen des entsprechenden [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) verwenden Sie die [**Point**](/uwp/api/windows.devices.geolocation.geocoordinate.point)-Eigenschaft der Geoposition-Geokoordinate. Weitere Informationen finden Sie unter [Get Current Location](get-location.md).

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

Beim Anzeigen der Geräteposition auf einer Karte sollten Sie für das Anzeigen von Grafiken und Festlegen des Zoomfaktors die Genauigkeit der Positionsdaten berücksichtigen. Weitere Informationen finden Sie unter [Richtlinien für standortabhängige apps](./guidelines-and-checklist-for-detecting-location.md).

### <a name="change-the-location-of-the-map"></a>Ändern der Kartenposition

Rufen Sie zum Ändern der Position, die in einer 2D-Karte angezeigt wird, eine der Überladungen der [**TrySetViewAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewasync)-Methode auf. Verwenden Sie diese Methode, um neue Werte für [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center), [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel), [**Heading**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) und [**Pitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitch) anzugeben. Darüber hinaus können Sie eine optionale Animation angeben, die beim Ändern der Ansicht angezeigt werden soll, indem Sie eine Konstante aus der Enumeration [**MapAnimationKind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) angeben.

Verwenden Sie zum Ändern des Standorts einer 3D-Karte stattdessen die [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync)-Methode. Weitere Informationen finden Sie unter [Anzeigen von Luftbild-3D-Ansichten](#3Dviews).

Rufen Sie die [**TrySetViewBoundsAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewboundsasync)-Methode zum Anzeigen der Inhalte eines [**GeoboundingBox**](/uwp/api/Windows.Devices.Geolocation.GeoboundingBox) auf der Karte auf. Diese Methode verwenden Sie beispielsweise, um eine Route oder einen Abschnitt einer Route auf der Karte anzuzeigen. Weitere Informationen finden Sie unter [Anzeigen von Routen und Directions on a map](routes-and-directions.md).

## <a name="change-the-appearance-of-a-map"></a>Ändern der Darstellung einer Karte

Um das Erscheinungsbild der Karte anzupassen, legen Sie die [**Stylesheet**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet) -Eigenschaft des Karten Steuer Elements auf eines der vorhandenen [**mapstylesheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) -Objekte fest.

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![Dunkle stilkarte](images/style-dark.png)

Sie können auch JSON verwenden, um benutzerdefinierte Stile zu definieren und diese JSON dann zum Erstellen eines [**mapstylesheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) -Objekts zu verwenden.

JSON für Stylesheets kann interaktiv mithilfe der Editor-Anwendung des [Karten Stylesheets](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) erstellt werden.

```csharp
myMap.StyleSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""settings"": {
            ""landColor"": ""#FFFFFF"",
            ""spaceColor"": ""#000000""
        },
        ""elements"": {
            ""mapElement"": {
                ""labelColor"": ""#000000"",
                ""labelOutlineColor"": ""#FFFFFF""
            },
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            },
            ""area"": {
                ""fillColor"": ""#EEEEEE""
            },
            ""political"": {
                ""borderStrokeColor"": ""#CCCCCC"",
                ""borderOutlineColor"": ""#00000000""
            }
        }
    }
");
```

![Benutzerdefinierte Stil Zuordnung](images/style-custom.png)

Die Referenz zum kompletten JSON-Eintrag finden Sie unter Übersicht über das [Karten Stylesheet](elements-of-map-style-sheet.md).

Sie können mit einem vorhandenen Blatt beginnen und dann JSON verwenden, um alle gewünschten Elemente zu überschreiben. Dieses Beispiel beginnt mit einem vorhandenen Stil und verwendet JSON, um nur die Farbe von Wasser Bereichen zu ändern.

```csharp
 MapStyleSheet \customSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""elements"": {
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            }
        }
    }
");

MapStyleSheet builtInSheet = MapStyleSheet.RoadDark();

myMap.StyleSheet = MapStyleSheet.Combine(new List<MapStyleSheet> { builtInSheet, customSheet });
```

![Stil Zuordnung kombinieren](images/style-combined.png)

>[!NOTE]
>Stile, die Sie im zweiten Stylesheet definieren, überschreiben die Stile in der ersten.

## <a name="set-orientation-and-perspective"></a>Ausrichtung und Perspektive festlegen

Vergrößern, verkleinern, drehen und kippen Sie die Kamera der Karte, um nur den rechten Winkel für den gewünschten Effekt zu erhalten. Probieren Sie diese Eigenschaften aus.

-   Legen Sie das **Zentrum** der Karte auf einen geografischen Punkt fest, indem Sie die Eigenschaft [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) festlegen.
-   Legen Sie die **Zoomstufe** der Karte fest, indem Sie die Eigenschaft [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) auf einen Wert zwischen 1 und 20 Grad festlegen.
-   Legen Sie die **Rotation** der Karte fest, indem Sie die Eigenschaft [**Heading**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) festlegen, wobei 0 oder 360 Grad = Nord, 90 = Ost, 180 = Süd und 270 =West ist.
-   Legen Sie die **Neigung** der Karte fest, indem Sie die Eigenschaft [**DesiredPitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.desiredpitch) auf einen Wert zwischen 0 und 65 Grad festlegen.

## <a name="show-and-hide-map-features"></a>Karten Features anzeigen und ausblenden

Ein-oder Ausblenden von Karten Features, wie z. b. Straßen und Meilensteine durch Festlegen der Werte der folgenden Eigenschaften von [**mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

* Zeigen Sie **Gebäude und Sehenswürdigkeiten** auf der Karte an, indem Sie die Eigenschaft [**LandmarksVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.landmarksvisible) aktivieren oder deaktivieren.

  > [!NOTE]
  > Sie können Gebäude ein-oder ausblenden, aber Sie können nicht verhindern, dass Sie drei Dimensionen anzeigen.  

* Zeigen Sie **Features für Fußgänger** auf der Karte an, wie öffentliche Treppen, indem Sie die Eigenschaft [**PedestrianFeaturesVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pedestrianfeaturesvisible) aktivieren oder deaktivieren.
* Zeigen Sie den **Verkehr** auf der Seite an, indem Sie die Eigenschaft [**TrafficFlowVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trafficflowvisible) aktivieren oder deaktivieren.
* Geben Sie an, ob das **Wasserzeichen** auf der Karte angezeigt wird, indem Sie die Eigenschaft [**WatermarkMode**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.watermarkmode) auf eine der [**MapWatermarkMode**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapWatermarkMode)-Konstanten festlegen.
* Zeigen Sie eine **Auto- oder Fußgängerroute** auf der Karte an, indem Sie [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) zur Sammlung [**Routes**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) des Kartensteuerelements hinzufügen. Weitere Informationen und ein Beispiel finden Sie unter [Anzeigen von Routen und Directions on a map](routes-and-directions.md).

Informationen zum Anzeigen von Ortsmarken, Formen und XAML-Steuerelementen in [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) finden Sie unter [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md).

## <a name="display-streetside-views"></a>Anzeigen von Streetside-Ansichten


Bei einer Streetside-Ansicht handelt es sich um die Straßenansicht eines Standorts, die über dem Kartensteuerelement angezeigt wird.

![Beispiel für eine Streetside-Ansicht des Kartensteuerelements](images/onlystreetside-730width.png)

Betrachten Sie die Vorgänge „innerhalb“ der Streetside-Ansicht getrennt von der ursprünglich im Kartensteuerelement angezeigten Karte. Wenn z. B. der Standort in der Streetside-Ansicht geändert wird, führt dies nicht zu einer Änderung der Position oder der Darstellung der Karte "unter" der Streetside-Ansicht. Nach dem Schließen der Streetside-Ansicht (durch Klicken auf **X** oben rechts im Steuerelement) bleibt die ursprüngliche Karte unverändert.

So zeigen Sie eine Streetside-Ansicht an

1.  Bestimmen Sie, ob auf dem Gerät auf der linken Seite auf der Seite " [**isstreetsidesupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.isstreetsidesupported)" unterstützt wird.
2.  Wenn Streetside-Ansichten unterstützt werden, erstellen Sie in der Nähe der angegebenen Position [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama), indem Sie [**FindNearbyAsync**](/uwp/api/windows.ui.xaml.controls.maps.streetsidepanorama.findnearbyasync) aufrufen.
3.  Bestimmen Sie, ob ein Panorama in der Nähe gefunden wurde, indem Sie überprüfen, ob [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) ungleich NULL ist.
4.  Wenn ein Panorama in der Nähe gefunden wurde, erstellen Sie eine [**StreetsideExperience**](/uwp/api/windows.ui.xaml.controls.maps.streetsideexperience) für die [**CustomExperience**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.customexperience)-Eigenschaft des Kartensteuerelements.

In diesem Beispiel wird gezeigt, wie Sie eine Streetside-Ansicht ähnlich wie in der Abbildung oben anzeigen.

**Hinweis**    Die Übersichtskarte wird nicht angezeigt, wenn das Karten Steuerelement zu klein ist.

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

<a id="3Dviews" />
## <a name="display-aerial-3d-views"></a>Anzeigen von 3D-Luftbildern


Mithilfe der Klasse [**MapScene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) können Sie eine 3D-Perspektive der Karte angeben. Die Kartenszene stellt die 3D-Ansicht dar, die in der Karte angezeigt wird. Die Klasse [**MapCamera**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapCamera) stellt die Position der Kamera dar, mit der eine solche Ansicht angezeigt würde.

![Diagramm mit MapCamera-Position und Kartenszenenposition](images/mapcontrol-techdiagram.png)

Damit Gebäude und andere Merkmale auf der Kartenoberfläche in 3D angezeigt werden, legen Sie die [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style)-Eigenschaft des Kartensteuerelements auf [**MapStyle.Aerial3DWithRoads**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle) fest. Dies ist ein Beispiel für eine 3D-Ansicht mit dem Stil **Aerial3DWithRoads**.

![Beispiel für eine 3D-Kartenansicht](images/only3d-730width.png)

So zeigen Sie eine 3D-Ansicht an

1.  Überprüfen Sie [**Is3DSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.is3dsupported), ob 3D-Sichten auf dem Gerät unterstützt werden.
2.  Wenn 3D-Sichten unterstützt werden, legen Sie die [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) -Eigenschaft des Karten Steuer Elements auf [**MapStyle. Aerial3DWithRoads**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)fest.
3.  Erstellen Sie ein [**mapscene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) -Objekt mit einer der vielen Methoden zum Erstellen **von** Methoden, z. b. " [**kreatefromlocationandradius**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromlocationandradius) " und " [**kreatefromcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromcamera)".
4.  Rufen Sie [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) auf, um die 3D-Ansicht anzuzeigen. Darüber hinaus können Sie eine optionale Animation angeben, die beim Ändern der Ansicht angezeigt werden soll, indem Sie eine Konstante aus der Enumeration [**MapAnimationKind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) angeben.

In diesem Beispiel wird das Anzeigen einer 3D-Ansicht gezeigt.

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 43.773251, Longitude = 11.255474};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## <a name="get-info-about-locations"></a>Informationen zu Speicherorten erhalten


Rufen Sie Informationen zu Speicherorten auf der Karte auf, indem Sie die folgenden Methoden des [**mapcontrol-Steuer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)Elements aufrufen.

-   [**Trygetlocationfromuffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getlocationfromoffset) -Methode: der geografische Speicherort, der dem angegebenen Punkt im Viewport des Karten Steuer Elements entspricht, wird abgerufen.
-   [**GetOffsetFromLocation**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getoffsetfromlocation)-Methode – Ruft den Punkt im Viewport des Kartensteuerelements ab, der dem angegebenen geografischen Standort entspricht.
-   [**IsLocationInView**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.islocationinview)-Methode – Bestimmt, ob der angegebene geografische Standort aktuell im Viewport des Kartensteuerelements sichtbar ist.
-   [**FindMapElementsAtOffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.findmapelementsatoffset)-Methode – Ruft die Elemente auf der Karte ab, die sich am angegebenen Punkt im Viewport des Kartensteuerelements befinden.

## <a name="handle-interaction-and-changes"></a>Behandeln von Interaktionen und Änderungen


Sie behandeln Benutzereingabegesten auf der Karte, indem Sie die folgenden Ereignisse von [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) behandeln. Überprüfen Sie die Werte der [**Speicherort**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.location) -und [**Positions**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.position) Eigenschaften von [**mapinputeventargs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapInputEventArgs), um Informationen über die geografische Position auf der Karte und die physische Position im Viewport zu erhalten, an der die Geste aufgetreten ist.

-   [**MapTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.maptapped)
-   [**MapDoubleTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapdoubletapped)
-   [**MapHolding**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapholding)

Sie stellen fest, ob die Karte geladen wird oder vollständig geladen wurde, indem Sie das [**LoadingStatusChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.loadingstatuschanged)-Ereignis des Steuerelements behandeln.

Sie behandeln Änderungen, die durch Ändern der Karteneinstellungen durch den Benutzer oder die App entstehen, indem Sie die folgenden Ereignisse von [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) behandeln. [Richtlinien für Karten]()

-   [**CenterChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.centerchanged)
-   [**HeadingChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.headingchanged)
-   [**PitchChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitchchanged)
-   [**ZoomLevelChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevelchanged)

## <a name="best-practice-recommendations"></a>Empfehlungen zu bewährten Methoden

-   Verwenden Sie für die Anzeige der Karte ausreichend Platz auf dem Bildschirm (oder den gesamten Bildschirm), sodass Benutzer beim Anzeigen geografischer Informationen keine übermäßigen Verschiebungen oder Zoomvorgänge ausführen müssen.

-   Wenn die Karte nur zum Anzeigen einer statischen, informativen Ansicht dient, ist möglicherweise eine kleinere Karte ausreichend. Wenn Sie eine kleinere, statische Karte verwenden, orientieren Sie sich für die Größe an der Benutzerfreundlichkeit. Die Karte sollte klein genug sein, um genügend Platz auf dem Bildschirm zu lassen, aber groß genug, um lesbar zu bleiben.

-   Betten Sie die interessanten Orte mithilfe von [**map elements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapelementsproperty) in die Kartenszene ein. Zusätzliche Informationen können als vorübergehende, die Kartenszene überlagernde Benutzeroberfläche angezeigt werden.

## <a name="related-topics"></a>Zugehörige Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Abrufen der aktuellen Position](get-location.md)
* [Entwurfsrichtlinien für Apps mit Positionsbestimmung](./guidelines-and-checklist-for-detecting-location.md)
* [Entwurfsrichtlinien für Karten]()
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)