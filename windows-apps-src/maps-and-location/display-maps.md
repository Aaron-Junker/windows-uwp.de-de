---
title: Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten
description: Sie können eine Karte als sog. *Popupkarte* in einem einfach ausblendbaren Fenster anzeigen oder in einem voll funktionsfähigen Kartensteuerelement.
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, karten, standort, kartensteuerelement, kartenansichten
ms.localizationpriority: medium
ms.openlocfilehash: 41d44f75c80e10123c21fc310bff46371ede0fca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641175"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten

Sie können eine Karte als sog. *Popupkarte* in einem einfach ausblendbaren Fenster anzeigen oder in einem voll funktionsfähigen Kartensteuerelement.

Laden Sie das [Kartenbeispiel](https://go.microsoft.com/fwlink/p/?LinkId=619977) herunter, um einige der in diesem Handbuch beschriebenen Funktionen auszuprobieren.

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>Anzeigen einer Popupkarte
Sie können Benutzern eine Karte in einem einfachen Popupfenster über, unter oder neben einem Benutzeroberflächenelement anzeigen oder in einem App-Bereich, in den der Benutzer tippt. Die Karte kann eine Stadt oder Adresse zeigen, die sich auf Informationen in Ihrer App bezieht.  

Diese Popupkarte zeigt die Stadt Seattle.

![Popupkarte mit der Stadt Seattle](images/placecard-city.png)

Es folgt der Code, der Seattle in einer Popupkarte unterhalb einer Schaltfläche anzeigt.

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

Diese Popupkarte zeigt den Standort der Space Needle in Seattle.

![Popupkarte mit dem Stanort der Space Needle](images/placecard-needle.png)

Es folgt der Code, der die Space Needle in einer Popupkarte unterhalb einer Schaltfläche anzeigt.

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

## <a name="display-map-in-a-control"></a>Anzeigen eines Kartensteuerelements

Verwenden Sie ein Kartensteuerelement, um in Ihrer App reichhaltige und anpassbare Kartendaten anzuzeigen. Mithilfe des Kartensteuerelements können Straßenkarten, Luftbilder, 3D-Ansichten, Wegbeschreibungen, Suchergebnisse und Verkehrsinformationen angezeigt werden. In einer Karte können Sie die Position des Benutzers, Wegbeschreibungen und interessante Orte anzeigen. Zudem kann eine Karte 3D-Luftbilder, Streetside-Ansichten, den Verkehr, öffentliche Verkehrsmittel und lokale Unternehmen enthalten.

Verwenden Sie ein Kartensteuerelement, wenn Sie in Ihrer App eine Karte benötigen, auf der Benutzer App-spezifische oder allgemeine geografische Informationen anzeigen können. Wenn die App ein Kartensteuerelement enthält, müssen Benutzer die App für diese Informationen nicht verlassen.

> [!NOTE]
>Wenn es Ihnen nichts ausmacht, dass Benutzer dafür die App verlassen, können Sie diese Informationen auch mit der Windows-Karten-App bereitstellen. Ihre App kann die Windows-Karten-App starten, um bestimmte Karten, Wegbeschreibungen und Suchergebnisse anzuzeigen. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://msdn.microsoft.com/library/windows/apps/mt228341).

### <a name="add-a-map-control-to-your-app"></a>Hinzufügen eines Kartensteuerelements zu einer App

Zeigen Sie eine Karte auf einer XAML-Seite durch Hinzufügen eines [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) an. Um **MapControl** zu verwenden, müssen Sie den Namespace [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) in der XAML-Seite oder im Code deklarieren. Wenn Sie das Steuerelement der Toolbox entnehmen, wird die Namespacedeklaration automatisch hinzugefügt. Wenn Sie **MapControl** manuell zur XAML-Seite hinzufügen, müssen Sie die Namespacedeklaration oben auf der Seite manuell hinzufügen.

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

Bevor Sie [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) und Kartendienste verwenden können, müssen Sie den Kartenauthentifizierungsschlüssel als Wert für die [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036)-Eigenschaft angeben. Ersetzen Sie in den vorherigen Beispielen `EnterYourAuthenticationKeyHere` durch den Schlüssel, den Sie über das [Bing Maps Developer Center](https://www.bingmapsportal.com/) abgerufen haben. Der Text **Warnung: Nicht angegebene MapServiceToken** weiterhin unterhalb des Steuerelements angezeigt werden soll, bis Sie angeben, dass den Maps-Authentifizierungsschlüssel. Weitere Informationen zum Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

## <a name="set-the-location-of-a-map"></a>Festlegen der aktuellen Kartenposition
Zeigen Sie die Karte mit einen beliebigen Standort an oder verwenden Sie den aktuellen Standort des Benutzers.  

### <a name="set-a-starting-location-for-the-map"></a>Festlegen einer Ausgangsposition für die Karte

Legen Sie die Position fest, die auf der Karte angezeigt werden soll, indem Sie die [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)-Eigenschaft von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) im Code angeben oder die Eigenschaft im XAML-Markup binden. Im folgenden Beispiel wird eine Karte mit Seattle in der Mitte angezeigt.

> [!NOTE]
> Da eine Zeichenfolge nicht in einen [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) konvertiert werden kann, können Sie nur dann einen Wert für die [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)-Eigenschaft im XAML-Markup festlegen, wenn Sie die Datenbindung verwenden. (Diese Einschränkung gilt auch für die angefügte Eigenschaft [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264).

 
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

Bevor die App auf die Position des Benutzers zugreifen kann, muss die App die [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)-Methode aufrufen. Zu diesem Zeitpunkt muss sich Ihre App im Vordergrund befinden, und **RequestAccessAsync** muss vom UI-Thread aufgerufen werden. Solange der Benutzer Ihrer App keinen Zugriff auf seine Position gewährt hat, kann Ihre App nicht auf Positionsdaten zugreifen.

Rufen Sie mit der [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)-Methode der Klasse [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) die aktuelle Position des Geräts ab (wenn die Position verfügbar ist). Zum Abrufen des entsprechenden [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) verwenden Sie die [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665)-Eigenschaft der Geoposition-Geokoordinate. Weitere Informationen finden Sie unter [Abrufen der aktuellen Position](get-location.md).

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

Beim Anzeigen der Geräteposition auf einer Karte sollten Sie für das Anzeigen von Grafiken und Festlegen des Zoomfaktors die Genauigkeit der Positionsdaten berücksichtigen. Weitere Informationen finden Sie unter [Richtlinien für Apps mit Standortbestimmung](https://msdn.microsoft.com/library/windows/apps/hh465148).

### <a name="change-the-location-of-the-map"></a>Ändern der Kartenposition

Rufen Sie zum Ändern der Position, die in einer 2D-Karte angezeigt wird, eine der Überladungen der [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060)-Methode auf. Verwenden Sie diese Methode, um neue Werte für [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005), [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068), [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) und [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044) anzugeben. Darüber hinaus können Sie eine optionale Animation angeben, die beim Ändern der Ansicht angezeigt werden soll, indem Sie eine Konstante aus der Enumeration [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) angeben.

Verwenden Sie zum Ändern des Standorts einer 3D-Karte stattdessen die [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296)-Methode. Weitere Informationen finden Sie unter [Anzeigen von 3D-Luftbildern](#3Dviews).

Rufen Sie die [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065)-Methode zum Anzeigen der Inhalte eines [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) auf der Karte auf. Diese Methode verwenden Sie beispielsweise, um eine Route oder einen Abschnitt einer Route auf der Karte anzuzeigen. Weitere Informationen finden Sie unter [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md).

## <a name="change-the-appearance-of-a-map"></a>Ändern des Aussehens einer Karte

Um das Aussehen und Verhalten der Karte anzupassen, legen Sie die [**StyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet)-Eigenschaft des Kartensteuerelements auf eines der vorhandenen [**MapStyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) Objekte fest.

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![Karte im dunklen Stil](images/style-dark.png)

Sie können auch JSON verwenden, um benutzerdefinierte Stile zu definieren und dann JSON zum Erstellen eines [**MapStyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) Objekt nutzen.

Stylesheet JSON erstellt werden kann, interaktiv mit dem [Zuordnung Formatvorlagen-Editor](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) Anwendung.

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

![Karte im benutzerdefinierten Stil](images/style-custom.png)

Die vollständige JSON-Referenz finden Sie unter [Karten-Stylesheet-Referenz](elements-of-map-style-sheet.md).

Können Sie mit einem vorhandenen Sheet beginnen und dann JSON verwenden, um alle gewünschten Elemente zu überschreiben. Dieses Beispiel beginnt mit einem vorhandenen Stil und verwendet JSON, um nur die Farbe der Wasserbereiche zu ändern.

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

![Kombinierter Kartenstil](images/style-combined.png)

>[!NOTE]
>Stile, die Sie in einem zweiten Stylesheet definieren, überschreiben die Stile in der ersten.

## <a name="set-orientation-and-perspective"></a>Festlegen von Ausrichtung und Perspektive

Vergrößern, verkleinern, drehen Sie und kippen Sie der Kamera der Karte, um den passen Winkel für den gewünschten Effekt zu erhalten. Versuchen Sie diese Eigenschaften:

-   Legen Sie das **Zentrum** der Karte auf einen geografischen Punkt fest, indem Sie die Eigenschaft [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) festlegen.
-   Legen Sie die **Zoomstufe** der Karte fest, indem Sie die Eigenschaft [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) auf einen Wert zwischen 1 und 20 Grad festlegen.
-   Legen Sie die **Rotation** der Karte fest, indem Sie die Eigenschaft [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) festlegen, wobei 0 oder 360 Grad = Nord, 90 = Ost, 180 = Süd und 270 =West ist.
-   Legen Sie die **Neigung** der Karte fest, indem Sie die Eigenschaft [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) auf einen Wert zwischen 0 und 65 Grad festlegen.

## <a name="show-and-hide-map-features"></a>Ein- und Ausblenden von Kartefeatures

Ausblenden und Anzeigen von Features wie Straßen und Orten, indem Sie die Werte der folgenden Eigenschaften von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) festlegen.

* Zeigen Sie **Gebäude und Sehenswürdigkeiten** auf der Karte an, indem Sie die Eigenschaft [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023) aktivieren oder deaktivieren.

  > [!NOTE]
  > Sie können Gebäude ein- oder ausblenden, aber Sie können nicht verhindern, dass sie dreidimensional erscheinen.  

* Zeigen Sie **Features für Fußgänger** auf der Karte an, wie öffentliche Treppen, indem Sie die Eigenschaft [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042) aktivieren oder deaktivieren.
* Zeigen Sie den **Verkehr** auf der Seite an, indem Sie die Eigenschaft [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055) aktivieren oder deaktivieren.
* Geben Sie an, ob das **Wasserzeichen** auf der Karte angezeigt wird, indem Sie die Eigenschaft [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) auf eine der [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749)-Konstanten festlegen.
* Zeigen Sie eine **Auto- oder Fußgängerroute** auf der Karte an, indem Sie [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) zur Sammlung [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) des Kartensteuerelements hinzufügen. Weitere Informationen und ein Beispiel finden Sie in [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md).

Informationen zum Anzeigen von Ortsmarken, Formen und XAML-Steuerelementen in [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) finden Sie unter [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md).

## <a name="display-streetside-views"></a>Anzeigen von Streetside-Ansichten


Bei einer Streetside-Ansicht handelt es sich um die Straßenansicht eines Standorts, die über dem Kartensteuerelement angezeigt wird.

![Beispiel für eine Streetside-Ansicht des Kartensteuerelements](images/onlystreetside-730width.png)

Betrachten Sie die Vorgänge „innerhalb“ der Streetside-Ansicht getrennt von der ursprünglich im Kartensteuerelement angezeigten Karte. Wenn z. B. der Standort in der Streetside-Ansicht geändert wird, führt dies nicht zu einer Änderung der Position oder der Darstellung der Karte "unter" der Streetside-Ansicht. Nach dem Schließen der Streetside-Ansicht (durch Klicken auf **X** oben rechts im Steuerelement) bleibt die ursprüngliche Karte unverändert.

So zeigen Sie eine Streetside-Ansicht an

1.  Überprüfen Sie mittels [**IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271), ob Streetside-Ansichten auf dem Gerät unterstützt werden.
2.  Wenn Streetside-Ansichten unterstützt werden, erstellen Sie in der Nähe der angegebenen Position [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360), indem Sie [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974361) aufrufen.
3.  Bestimmen Sie, ob ein Panorama in der Nähe gefunden wurde, indem Sie überprüfen, ob [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) ungleich NULL ist.
4.  Wenn ein Panorama in der Nähe gefunden wurde, erstellen Sie eine [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356) für die [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263)-Eigenschaft des Kartensteuerelements.

In diesem Beispiel wird gezeigt, wie Sie eine Streetside-Ansicht ähnlich wie in der Abbildung oben anzeigen.

**Beachten Sie**  die Übersichtskarte wird nicht angezeigt, wenn das Map-Steuerelement zu klein festgelegt wird.

 

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


Mithilfe der Klasse [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) können Sie eine 3D-Perspektive der Karte angeben. Die Kartenszene stellt die 3D-Ansicht dar, die in der Karte angezeigt wird. Die Klasse [**MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) stellt die Position der Kamera dar, mit der eine solche Ansicht angezeigt würde.

![Diagramm mit MapCamera-Position und Kartenszenenposition](images/mapcontrol-techdiagram.png)

Damit Gebäude und andere Merkmale auf der Kartenoberfläche in 3D angezeigt werden, legen Sie die [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051)-Eigenschaft des Kartensteuerelements auf [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127) fest. Dies ist ein Beispiel für eine 3D-Ansicht mit dem Stil **Aerial3DWithRoads**.

![Beispiel für eine 3D-Kartenansicht](images/only3d-730width.png)

So zeigen Sie eine 3D-Ansicht an

1.  Überprüfen Sie mittels [**Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265), ob 3D-Ansichten auf dem Gerät unterstützt werden.
2.  Wenn 3D-Ansichten unterstützt werden, legen Sie die [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051)-Eigenschaft des Kartensteuerelements auf [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127) fest.
3.  Erstellen Sie ein [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329)-Objekt mithilfe einer der zahlreichen **CreateFrom**-Methoden, z. B. [**CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974336) und [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974334).
4.  Rufen Sie [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) auf, um die 3D-Ansicht anzuzeigen. Darüber hinaus können Sie eine optionale Animation angeben, die beim Ändern der Ansicht angezeigt werden soll, indem Sie eine Konstante aus der Enumeration [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) angeben.

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

## <a name="get-info-about-locations"></a>Informationen über Standorte abrufen


Rufen Sie Informationen zu Positionen auf der Karte ab, indem Sie die folgenden Methoden von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) aufrufen.

-   [**TryGetLocationFromOffset** ](https://msdn.microsoft.com/library/windows/apps/dn637016) -Methode: Get den geografischen Standort, der dem angegebenen Punkt im Anzeigebereich das Map-Steuerelement entspricht.
-   [**GetOffsetFromLocation** ](https://msdn.microsoft.com/library/windows/apps/dn637018) -Methode: Get der Punkt relativ zum Viewport Kartensteuerelement, das den angegebenen geografischen Standort entspricht.
-   [**IsLocationInView** ](https://msdn.microsoft.com/library/windows/apps/dn637022) -Methode: bestimmen, ob die angegebene geografische Standort derzeit im Viewport des Kartensteuerelements sichtbar ist.
-   [**FindMapElementsAtOffset** ](https://msdn.microsoft.com/library/windows/apps/dn637014) -Methode: Get, die die Elemente auf der Karte befindet sich am angegebenen Punkt relativ zum Viewport des Kartensteuerelements.

## <a name="handle-interaction-and-changes"></a>Behandeln von Interaktionen und Änderungen


Sie behandeln Benutzereingabegesten auf der Karte, indem Sie die folgenden Ereignisse von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) behandeln. Rufen Sie Informationen zum geografischen Standort auf der Karte und der physischen Position im Viewport ab, an der die Geste ausgeführt wurde, indem Sie die Werte der [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637091)-Eigenschaft und der [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637093)-Eigenschaft von [**MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637090) überprüfen.

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

Sie stellen fest, ob die Karte geladen wird oder vollständig geladen wurde, indem Sie das [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028)-Ereignis des Steuerelements behandeln.

Sie behandeln Änderungen, die durch Ändern der Karteneinstellungen durch den Benutzer oder die App entstehen, indem Sie die folgenden Ereignisse von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) behandeln. [Richtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## <a name="best-practice-recommendations"></a>Bewährte Methoden und Empfehlungen

-   Verwenden Sie für die Anzeige der Karte ausreichend Platz auf dem Bildschirm (oder den gesamten Bildschirm), sodass Benutzer beim Anzeigen geografischer Informationen keine übermäßigen Verschiebungen oder Zoomvorgänge ausführen müssen.

-   Wenn die Karte nur zum Anzeigen einer statischen, informativen Ansicht dient, ist möglicherweise eine kleinere Karte ausreichend. Wenn Sie eine kleinere, statische Karte verwenden, orientieren Sie sich für die Größe an der Benutzerfreundlichkeit. Die Karte sollte klein genug sein, um genügend Platz auf dem Bildschirm zu lassen, aber groß genug, um lesbar zu bleiben.

-   Betten Sie die interessanten Orte mithilfe von [**map elements**](https://msdn.microsoft.com/library/windows/apps/dn637034) in die Kartenszene ein. Zusätzliche Informationen können als vorübergehende, die Kartenszene überlagernde Benutzeroberfläche angezeigt werden.

## <a name="related-topics"></a>Verwandte Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Abrufen der aktuellen Position](get-location.md)
* [Entwurfsrichtlinien für Apps mit Positionsbestimmung](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [Entwurfsrichtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)
