---
title: Anzeigen von interessanten Orten (POI) auf einer Karte
description: Mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen können Sie interessante Orte (Points of Interest, POI) auf einer Karte hinzufügen.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.date: 08/11/2017
ms.topic: article
keywords: Windows 10, UWP, map, Location, Pushpins
ms.localizationpriority: medium
ms.openlocfilehash: c27132c0728c85238b80e710c62d2e733ee1dd5d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155804"
---
# <a name="display-points-of-interest-on-a-map"></a>Relevante Punkte in einer Karte anzeigen

Mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen können Sie interessante Orte (Points of Interest, POI) auf einer Karte hinzufügen. Ein POI ist ein Punkt auf der Karte, der Orte angibt, die von Interesse sind. Beispiele sind die Position eines Geschäfts, eines Orts oder eines Freundes.

Wenn Sie weitere Informationen zum Anzeigen von POI in Ihrer APP haben, laden Sie das folgende Beispiel aus dem Repository [Windows-Universal-Samples](https://github.com/Microsoft/Windows-universal-samples) auf GitHub: [universelle Windows-Plattform (UWP) map Sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)herunter.

Zeigen Sie Pushpins, Bilder und Formen auf der Karte an, indem Sie [**mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)-, [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)-,  [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)-und [**mappolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) -Objekte zu einer **mapelements** -Auflistung eines [**mapelementslayer**](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) -Objekts hinzufügen. Fügen Sie dann dieses Ebenenobjekt der **Layers** Ebenenauflistung eines Karten Steuer Elements hinzu.

>[!NOTE]
> In früheren Versionen haben Sie in dieser Anleitung gezeigt, wie Sie der [**mapelements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) -Auflistung Kartenelemente hinzufügen. Obwohl Sie diesen Ansatz weiterhin verwenden können, werden einige der Vorteile des neuen Karten Ebenen-Modells übersehen. Weitere Informationen finden Sie im Abschnitt [Arbeiten mit Ebenen](#layers) dieses Handbuchs.

Sie können auch XAML-Benutzeroberflächen Elemente wie z. b. eine [**Schaltfläche**](/uwp/api/Windows.UI.Xaml.Controls.Button), ein [**HyperlinkButton**](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)oder einen [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) in der Zuordnung anzeigen, indem Sie Sie dem [**mapitemscontrol-Steuer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl) Element [**oder als unter**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) geordnete Elemente des [**mapcontrol-Steuer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)Elements hinzufügen.

Wenn Sie eine große Anzahl von Elementen auf der Karte platzieren möchten, sollten Sie [nebeneinander angeordnete Bilder auf der Karte überlagern](overlay-tiled-images.md). Weitere Informationen zum Anzeigen von Straßen auf der Karte finden Sie unter [Anzeigen von Routen und Anleitungen](routes-and-directions.md) .

## <a name="add-a-pushpin"></a>Hinzufügen einer PushPin

Verwenden Sie die Klasse [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon), um Bilder wie eine Ortsmarke mit optionalem Text auf der Karte anzuzeigen. Sie können das Standardbild akzeptieren oder mit der [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image)-Eigenschaft ein benutzerdefiniertes Bild bereitstellen. Die folgende Abbildung zeigt das Standardbild für ein **MapIcon** ohne festgelegten Wert für die Eigenschaft [**Title**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) mit einem kurzen Titel, einem langen Titel und einem sehr langen Titel.

![Beispiel für MapIcon mit Titeln unterschiedlicher Länge](images/mapctrl-mapicons.png)

Im folgenden Beispiel wird einer Karte von Seattle ein [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) mit Standardbild und optionalem Titel hinzugefügt, um den Standort der Space Needle anzugeben. Außerdem wird die Karte auf dem Symbol zentriert und vergrößert. Allgemeine Informationen zum Verwenden des Karten Steuer Elements finden Sie unter [Anzeigen von Karten mit 2D-, 3D-und Streetside-Ansichten](display-maps.md).

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

In diesem Beispiel wird der folgende interessante Ort (POI) auf der Karte angezeigt (mit dem Standardbild in der Mitte).

![Karte mit MapIcon](images/displaypoidefault.png)

Die folgende Codezeile zeigt [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) mit einem benutzerdefinierten Bild an, das im Ordner „Assets“ (Ressourcen) des Projekts gespeichert wurde. Die Eigenschaft [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) von **MapIcon** erwartet einen Wert vom Typ [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Dieser Typ erfordert die Anweisung **using** für den Namespace [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Wenn Sie das gleiche Bild für mehrere Kartensymbole verwenden, deklarieren Sie [**randomaccessstreamreference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) auf der Seiten-oder App-Ebene, um die beste Leistung zu erzielen.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Berücksichtigen Sie beim Arbeiten mit der [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)-Klasse Folgendes:

-   Die [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image)-Eigenschaft unterstützt eine maximale Bildgröße von 2048 x 2048 Pixeln.
-   Standardmäßig wird die Anzeige des Bilds für das Kartensymbol nicht garantiert. Es wird möglicherweise ausgeblendet, wenn es andere Elemente oder Bezeichnungen auf der Karte verdeckt. Um die Sichtbarkeit beizubehalten, legen Sie die Eigenschaft " [**collisionverhaltenswunsch**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.collisionbehaviordesired) " des Karten Symbols auf [**mapelementcollisionbehavior. restvisible**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapElementCollisionBehavior)fest.
-   Die Anzeige des optionalen [**Title**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) für [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) wird nicht garantiert. Wenn der Text nicht angezeigt wird, verkleinern Sie den Wert der [**Zoomlevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) -Eigenschaft des [**mapcontrol-Steuer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)Elements.
-   Wenn Sie ein [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)-Bild anzeigen, das auf eine bestimmte Position auf der Karte hinweist, z. B. eine Ortsmarkierung oder ein Pfeil, sollten Sie in Erwägung ziehen, den Wert der [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.normalizedanchorpoint)-Eigenschaft auf den ungefähren Standort des Zeigers auf dem Bild festzulegen. Wenn Sie den Wert von **NormalizedAnchorPoint** beim Standardwert (0, 0) belassen, der die obere linke Ecke des Bilds darstellt, führen Änderungen am [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) der Karte möglicherweise dazu, dass das Bild auf eine andere Position zeigt.
-   Wenn Sie nicht explizit eine [Höhen](/uwp/api/windows.devices.geolocation.basicgeoposition) -und " [altitudereferencesystem](/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem)" festlegen, wird das [**mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) auf der-Oberfläche platziert.

## <a name="add-a-3d-pushpin"></a>3D-PushPin hinzufügen

Sie können dreidimensionale Objekte auf einer Karte hinzufügen. Verwenden Sie die [MapModel3D](/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) -Klasse, um ein 3D-Objekt aus einer [3MF-Datei (3D-Produktions Format)](https://3mf.io/specification/) zu importieren.

In diesem Bild werden 3D-Kaffeetassen verwendet, um die Orte von Kaffee Geschäften in einer Umgebung zu markieren.

![Bechern für Zuordnungen](images/mugs.png)

Der folgende Code fügt der Karte einen Kaffeebecher hinzu, indem Sie eine 3MF-Datei importieren. Um dies zu gewährleisten, fügt dieser Code das Bild in den Mittelpunkt der Karte ein, aber der Code würde das Bild wahrscheinlich einem bestimmten Speicherort hinzufügen.

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>Hinzufügen eines Image

Zeigen Sie große Bilder an, die sich auf Karten Orte beziehen, z. b. ein Bild eines Restaurants oder eines wegzeichens. Wenn Benutzer verkleinern, verkleinert sich das Bild proportional in der Größe, um dem Benutzer die Anzeige mehrerer Karten zu ermöglichen. Dies unterscheidet sich [**von einem mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) , das eine bestimmte Position kennzeichnet, in der Regel klein ist und die gleiche Größe wie die Benutzer in einer Karte vergrößern und verkleinern kann.

![Mapbillboard-Bild](images/map-billboard.png)

Der folgende Code zeigt das [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) , das in der obigen Abbildung dargestellt ist.

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

Es gibt drei Teile dieses Codes, die etwas näher untersucht werden müssen: das Image, die Referenz Kamera und die [**normalizedanchorpoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) -Eigenschaft.

### <a name="image"></a>Bild

Dieses Beispiel zeigt ein benutzerdefiniertes Image, das im Ordner " **Assets** " des Projekts gespeichert ist. Die [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) -Eigenschaft von [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) erwartet einen Wert vom Typ [**randomaccessstreamreference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Dieser Typ erfordert die Anweisung **using** für den Namespace [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Wenn Sie das gleiche Bild für mehrere Kartensymbole verwenden, deklarieren Sie [**randomaccessstreamreference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) auf der Seiten-oder App-Ebene, um die beste Leistung zu erzielen.

### <a name="reference-camera"></a>Referenz Kamera

 Da ein [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) -Bild beim Ändern der [**Zoomstufe**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) der Zuordnung horizontal hoch-und herunterskaliert wird, ist es wichtig, zu definieren, wo in diesem [**Zoomlevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) das Bild in einer normalen 1-stufigen Skala angezeigt wird. Diese Position wird in der Referenz Kamera von [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)definiert. um Sie festzulegen, müssen Sie ein [**mapcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) -Objekt an den Konstruktor von [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)übergeben.

 Sie können die gewünschte Position in einem [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint)definieren und dann mit diesem [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint) ein [**mapcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) -Objekt erstellen.  In diesem Beispiel verwenden wir jedoch nur das [**mapcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) -Objekt, das von der [**actualcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) -Eigenschaft des Map-Steuer Elements zurückgegeben wird. Dies ist die interne Kamera der Karte. Die aktuelle Position dieser Kamera wird zur Position der Verweis Kamera. die Position, an der das [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) -Bild bei einer 1X-Skala angezeigt wird.

 Wenn Ihre APP den Benutzern die Möglichkeit bietet, die Zuordnung zu vergrößern, wird die Größe des Bilds verringert, da die interne Karte der Maps in der Höhe zunimmt, während das Bild mit der 1X-Skala an der Position der Referenz Kamera bleibt.

### <a name="normalizedanchorpoint"></a>Normalizedanchorpoint

Der [**normalizedanchorpoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) ist der Punkt des Bilds, das mit der [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) -Eigenschaft von [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)verankert ist. Der Punkt 0,5, 1 ist die untere Mitte des Bilds. Da wir die [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) -Eigenschaft von [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) auf den Mittelpunkt des Steuer Elements der Karte festgelegt haben, wird die untere Mitte des Bilds in der Mitte des Maps-Steuer Elements verankert. Wenn das Bild direkt über einen Punkt zentriert angezeigt werden soll, legen Sie [**normalizedanchorpoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) auf 0,5, 0,5 fest.  

## <a name="add-a-shape"></a>Hinzufügen einer Form

Mithilfe der Klasse [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon) können Sie eine Form mit mehreren Punkten auf der Karte anzeigen. Im folgenden Beispiel (aus dem [Beispiel zu UWP-Karten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)) wird ein rotes Feld mit blauem Rahmen auf der Karte angezeigt.

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>Hinzufügen einer Linie


Mithilfe der Klasse [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) können Sie eine Linie auf der Karte anzeigen. Im folgenden Beispiel (aus dem [Beispiel zu UWP-Karten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)) wird eine gestrichelte Linie auf der Karte angezeigt.

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>Hinzufügen von XAML

Zeigen Sie benutzerdefinierte UI-Elemente mit XAML auf der Karte an. Positionieren Sie XAML auf der Karte, indem Sie die Position und den normalisierten Ankerpunkt für den XAML-Code angeben.

-   Rufen Sie [**SetLocation**](/windows/desktop/tablet/icontextnode-setlocation) auf, um die Position auf der Karte festzulegen, an der der XAML-Code platziert werden soll.
-   Legen Sie den relativen Speicherort für den XAML-Code fest, der der angegebenen Position entspricht, indem Sie [**setnormalizedanchorpoint**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setnormalizedanchorpoint)aufrufen.

Im folgenden Beispiel wird einer Karte von Seattle das XAML-Steuerelement [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) hinzugefügt, um den Standort der Space Needle anzugeben. Außerdem wird die Karte in diesem Bereich zentriert und vergrößert. Allgemeine Informationen zum Verwenden des Karten Steuer Elements finden Sie unter [Anzeigen von Karten mit 2D-, 3D-und Streetside-Ansichten](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

In diesem Beispiel wird ein blauer Rahmen auf der Karte angezeigt.

![Screenshot eines XAML-Elements, das am interessanten Ort auf der Karte angezeigt wird](images/displaypoixaml.png)

Die nächsten Beispiele zeigen, wie XAML-UI-Elemente direkt im XAML-Markup der Seite mithilfe der Datenbindung hinzugefügt werden. Wie bei anderen XAML-Elementen zur Anzeige von Inhalten auch, stellt [**Children**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) die standardmäßige Inhaltseigenschaft von [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) dar und muss nicht explizit im XAML-Markup angegeben werden.

Dieses Beispiel zeigt, wie zwei XAML-Steuerelemente als implizite untergeordnete Elemente des [**mapcontrol-Steuer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)Elements angezeigt werden. Diese Steuerelemente werden in der Karte an den Daten gebundenen Speicherorten angezeigt.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

Legen Sie diese Speicherorte fest, indem Sie in der Code Behind-Dateieigenschaften verwenden.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

Dieses Beispiel zeigt, wie zwei XAML-Steuerelemente angezeigt werden, die in einem [**mapitemscontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl)-Element enthalten sind. Diese Steuerelemente werden in der Karte an den Daten gebundenen Speicherorten angezeigt.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

Dieses Beispiel zeigt eine Auflistung von XAML-Elementen an, die an ein [**mapitemscontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl)-Element gebunden sind.

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

Die- ``ItemsSource`` Eigenschaft im obigen Beispiel wird in der Code Behind-Datei an eine Eigenschaft vom Typ [IList](/dotnet/api/system.collections.ilist) gebunden.

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>Arbeiten mit Ebenen

In den Beispielen in diesem Handbuch werden Elemente zu einer [MapElementLayers](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) -Auflistung hinzugefügt. Anschließend zeigen Sie, wie diese Auflistung der **Ebenen** -Eigenschaft des Map-Steuer Elements hinzugefügt wird. In früheren Versionen haben Sie in dieser Anleitung gezeigt, wie der [**mapelements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) -Auflistung Kartenelemente wie folgt hinzugefügt werden:

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

Obwohl Sie diesen Ansatz weiterhin verwenden können, werden einige der Vorteile des neuen Karten Ebenen-Modells übersehen. Indem Sie die Elemente in Ebenen gruppieren, können Sie jede Ebene unabhängig voneinander bearbeiten. Beispielsweise verfügt jede Ebene über einen eigenen Satz an Ereignissen, sodass Sie auf ein Ereignis in einer bestimmten Ebene antworten und eine Aktion ausführen können, die speziell für das Ereignis gilt.

Außerdem können Sie XAML direkt an einen [maplayer](/uwp/api/windows.ui.xaml.controls.maps.maplayer)binden. Dies ist etwas, das Sie nicht mit der [mapelements](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)  -Auflistung tun können.

Eine Möglichkeit besteht darin, eine Ansichts Modell Klasse, Code-Behind-XAML-Seite und eine XAML-Seite zu verwenden.

### <a name="view-model-class"></a>Modell Klasse anzeigen

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>Code Behind-XAML-Seite

Verbinden Sie die Ansichts Modell Klasse mit der Code Behind-Seite.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>XAML-Seite

Binden Sie in der XAML-Seite an die-Eigenschaft in der Ansichts Modell Klasse, die die Ebene zurückgibt.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>Zugehörige Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Entwurfsrichtlinien für Karten](./display-maps.md)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)
* [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)
* [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline)