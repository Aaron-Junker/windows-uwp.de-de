---
title: Anzeigen von interessanten Orten (POI) auf einer Karte
description: Mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen können Sie interessante Orte (Points of Interest, POI) auf einer Karte hinzufügen.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.date: 08/11/2017
ms.topic: article
keywords: windows 10, uwp, karten, standort, ortsmarken
ms.localizationpriority: medium
ms.openlocfilehash: 2aca8f4daea39a190af4dd1007a6b961198994dd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370544"
---
# <a name="display-points-of-interest-on-a-map"></a>Anzeigen von interessanten Orten (POI) auf einer Karte

Mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen können Sie interessante Orte (Points of Interest, POI) auf einer Karte hinzufügen. Ein POI ist ein Punkt auf der Karte, der Orte angibt, die von Interesse sind. Beispiele sind die Position eines Geschäfts, eines Orts oder eines Freundes.

Weitere Informationen zum Anzeigen von POI in Ihrer app Laden Sie im folgende Beispiel aus der [Windows-Universal-Samples Repository](https://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub: [Universelle Windows-Plattform (UWP) Map Beispiel](https://go.microsoft.com/fwlink/p/?LinkId=619977).

Zeigen Sie die Markiernadeln, Bilder und Formen auf der Karte an, indem Sie die Objekte [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon), [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard),  [**MapPolygon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon) und [**MapPolyline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) zu der **MapElements**-Sammlung eines [**MapElementsLayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer)-Objekts hinzufügen. Fügen Sie dann der **Layers**-Sammlung eines Kartensteuerelements das Layer-Objekt hinzu.

>[!NOTE]
> In früheren Versionen wurde in dieser Anleitung gezeigt, wie Sie der [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)-Sammlung Kartenelemente hinzufügen. Sie können diesen Ansatz weiterhin verwenden, doch werden Ihnen dann einige der Vorteile des neuen Kartenschichtenmodells nicht zur Verfügung stehen. Weitere Informationen hierzu finden Sie im Abschnitt [Arbeiten mit Schichten](#layers) dieser Anleitung.

Sie können auch XAML-Benutzeroberflächenelemente wie [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) oder [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) auf der Karte einsetzen, indem Sie sie zu [**MapItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl) hinzufügen oder als [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) für das [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) verwenden.

Wenn Sie eine große Anzahl von Elementen auf der Karte platzieren möchten, sollten Sie [nebeneinander angeordnete Bilder auf der Karte überlagern](overlay-tiled-images.md). Informationen zum Anzeigen von Straßen auf der Karte finden Sie unter [Anzeigen von Routen und Wegbeschreibungen](routes-and-directions.md).

## <a name="add-a-pushpin"></a>Hinzufügen einer Markiernadel

Verwenden Sie die Klasse [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon), um Bilder wie eine Ortsmarke mit optionalem Text auf der Karte anzuzeigen. Sie können das Standardbild akzeptieren oder mit der [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.image)-Eigenschaft ein benutzerdefiniertes Bild bereitstellen. Die folgende Abbildung zeigt das Standardbild für ein **MapIcon** ohne festgelegten Wert für die Eigenschaft [**Title**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) mit einem kurzen Titel, einem langen Titel und einem sehr langen Titel.

![Beispiel für MapIcon mit Titeln unterschiedlicher Länge](images/mapctrl-mapicons.png)

Im folgenden Beispiel wird einer Karte von Seattle ein [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) mit Standardbild und optionalem Titel hinzugefügt, um den Standort der Space Needle anzugeben. Außerdem wird die Karte auf dem Symbol zentriert und vergrößert. Allgemeine Informationen zur Verwendung des Kartensteuerelements finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

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

Die folgende Codezeile zeigt [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) mit einem benutzerdefinierten Bild an, das im Ordner „Assets“ (Ressourcen) des Projekts gespeichert wurde. Die Eigenschaft [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) von **MapIcon** erwartet einen Wert vom Typ [**RandomAccessStreamReference**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Dieser Typ erfordert die Anweisung **using** für den Namespace [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Wenn Sie dasselbe Bild für mehrere Kartensymbole verwenden, deklarieren Sie [**RandomAccessStreamReference**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) auf Seiten- oder App-Ebene, um eine optimale Leistung zu erzielen.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Berücksichtigen Sie beim Arbeiten mit der [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)-Klasse Folgendes:

-   Die [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.image)-Eigenschaft unterstützt eine maximale Bildgröße von 2048 x 2048 Pixeln.
-   Standardmäßig wird die Anzeige des Bilds für das Kartensymbol nicht garantiert. Es wird möglicherweise ausgeblendet, wenn es andere Elemente oder Bezeichnungen auf der Karte verdeckt. Damit es sichtbar bleibt, legen Sie die [**CollisionBehaviorDesired**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.collisionbehaviordesired)-Eigenschaft des Kartensymbols auf [**MapElementCollisionBehavior.RemainVisible**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapElementCollisionBehavior) fest.
-   Die Anzeige des optionalen [**Title**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) für [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) wird nicht garantiert. Wenn der Text nicht angezeigt wird, verkleinern Sie die Ansicht, indem Sie den Wert der [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel)-Eigenschaft von [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) verringern.
-   Wenn Sie ein [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)-Bild anzeigen, das auf eine bestimmte Position auf der Karte hinweist, z. B. eine Ortsmarkierung oder ein Pfeil, sollten Sie in Erwägung ziehen, den Wert der [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon.normalizedanchorpoint)-Eigenschaft auf den ungefähren Standort des Zeigers auf dem Bild festzulegen. Wenn Sie den Wert von **NormalizedAnchorPoint** beim Standardwert (0, 0) belassen, der die obere linke Ecke des Bilds darstellt, führen Änderungen am [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) der Karte möglicherweise dazu, dass das Bild auf eine andere Position zeigt.
-   Wenn Sie [Altitude](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.basicgeoposition) und [AltitudeReferenceSystem](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem) nicht explizit festlegen, wird [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) auf der Oberfläche platziert.

## <a name="add-a-3d-pushpin"></a>Hinzufügen einer 3D-Markiernadel

Sie können dreidimensionale Objekte auf einer Karte hinzufügen. Verwenden Sie die Klasse [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d), um ein 3D-Objekt aus einer [3D Manufacturing Format (3MF)](https://3mf.io/specification/)-Datei zu importieren.

Diese Abbildung verwendet 3D-Kaffeekannen, um die Standorte von Cafés der Umgebung zu markieren.

![Tassen auf der Karte](images/mugs.png)

Der folgende Code fügt der Karte eine Kaffeetasse hinzu, die aus einer 3MF-Datei importiert wird. Dieser Code fügt das Bild der Einfachheit halber in die Mitte der Karte ein. In Ihrem Code würden Sie das Bild wahrscheinlich an einer bestimmten Position einfügen.

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

## <a name="add-an-image"></a>Hinzufügen eines Bilds

Zeigen Sie große Bilder an, die im Zusammenhang mit Kartenpositionen (z. B. ein Bild von einem Restaurant oder einer Sehenswürdigkeit) stehen. Wenn der Benutzer verkleinert, wird das Bild proportional größer, damit der Benutzer mehr von der Karte anzeigen kann. Dies ist etwas anders als ein [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon), das einen bestimmten Ort kennzeichnet. Es ist in der Regel klein und bleibt bei der gleichen Größe wenn der Benutzer eine Karte verkleinert.

![MapBillboard-Bild](images/map-billboard.png)

Der folgende Code zeigt das [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) in der Abbildung oben.

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

Es gibt drei Teile dieses Codes sollte etwas näher zu untersuchen: Das Bild, Kamera Verweis an, und die [ **NormalizedAnchorPoint** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) Eigenschaft.

### <a name="image"></a>Bild

In diesem Beispiel wird ein benutzerdefiniertes Bild im **Assets** Ordner des Projekts gespeichert. Die Eigenschaft [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) von [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) erwartet einen Wert vom Typ [**RandomAccessStreamReference**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Dieser Typ erfordert die Anweisung **using** für den Namespace [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Wenn Sie dasselbe Bild für mehrere Kartensymbole verwenden, deklarieren Sie [**RandomAccessStreamReference**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) auf Seiten- oder App-Ebene, um eine optimale Leistung zu erzielen.

### <a name="reference-camera"></a>Referenzkamera

 Da ein [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)-Bild skaliert sobald sich das [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) der Karte ändert, es ist wichtig zu definieren, bei welchem [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) das Bild mit einer normalen 1x-Skalierung angezeigt wird. Die Position ist in der Referenzkamera des [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) definiert. Um sie festzulegen müssen Sie ein [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) Objekt an den Konstruktor des [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) übergeben.

 Können Sie die gewünschte Position in einem [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) definieren, und diesen [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) zum Erstellen eines [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) Objekts verwenden.  In diesem Beispiel verwenden wir jedoch das [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera)Objekt, das von der [**ActualCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera)-Eigenschaft des Kartensteuerelements zurückgegeben wird. Hierbei handelt es sich um die interne Kamera der Karte. Die aktuelle Position der Kamera wird die Referenzkameraposition (die Position, in der das [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) Bild mit 1x Skalierung angezeigt wird.

 Wenn Ihre App den Benutzern die Möglichkeit gibt, die Karte zu verkleinern, verringert das Bild die Größe, da die interne Kamera der Karte steigt während das Bild bei der 1x-Skalierung an der Referenzkameraposition verbleibt.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

Der [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) ist der Punkt des Bilds, das an der [**Speicherort**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) Eigenschaft des [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) verankert ist. Der Punkt 0,5,1 ist die untere Mitte des Bilds. Da wir die [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) Eigenschaft des [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)auf die untere Mitte des Kartensteuerelements festgelegt haben, wir die untere Mitte des Bilds an der unteren Mitte des Kartensteuerelements verankert. Wenn das Bild direkt über einem Punkt zentriert angezeigt werden soll, müssen Sie [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) auf 0.5,0.5 festlegen.  

## <a name="add-a-shape"></a>Hinzufügen einer Form

Mithilfe der Klasse [**MapPolygon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon) können Sie eine Form mit mehreren Punkten auf der Karte anzeigen. Im folgenden Beispiel (aus dem [Beispiel zu UWP-Karten](https://go.microsoft.com/fwlink/p/?LinkId=619977)) wird ein rotes Feld mit blauem Rahmen auf der Karte angezeigt.

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


Mithilfe der Klasse [**MapPolyline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) können Sie eine Linie auf der Karte anzeigen. Im folgenden Beispiel (aus dem [Beispiel zu UWP-Karten](https://go.microsoft.com/fwlink/p/?LinkId=619977)) wird eine gestrichelte Linie auf der Karte angezeigt.

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

-   Rufen Sie [**SetLocation**](https://docs.microsoft.com/windows/desktop/tablet/icontextnode-setlocation) auf, um die Position auf der Karte festzulegen, an der der XAML-Code platziert werden soll.
-   Rufen Sie [**SetNormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setnormalizedanchorpoint) auf, um die relative Position des XAML-Codes festzulegen, die der angegebenen Position entspricht.

Im folgenden Beispiel wird einer Karte von Seattle das XAML-Steuerelement [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) hinzugefügt, um den Standort der Space Needle anzugeben. Außerdem wird die Karte in diesem Bereich zentriert und vergrößert. Allgemeine Informationen zur Verwendung des Kartensteuerelements finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

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

Die nächsten Beispiele zeigen, wie XAML-UI-Elemente direkt im XAML-Markup der Seite mithilfe der Datenbindung hinzugefügt werden. Wie bei anderen XAML-Elementen zur Anzeige von Inhalten auch, stellt [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) die standardmäßige Inhaltseigenschaft von [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) dar und muss nicht explizit im XAML-Markup angegeben werden.

In diesem Beispiel wird gezeigt, wie zwei XAML-Steuerelemente als implizite untergeordnete Objekte von [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) angezeigt werden. Diese Steuerelemente werden auf der Karte an den datengebundenen Positionen angezeigt

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

Legen Sie diese Positionen mithilfe der Eigenschaften in der CodeBehind-Datei fest.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

Dieses Beispiel zeigt, wie zwei XAML-Steuerelemente aus einem [**MapItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl)angezeigt werden. Diese Steuerelemente werden auf der Karte an den datengebundenen Positionen angezeigt.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

In diesem Beispiel wird eine Sammlung von XAML-Elementen angezeigt, die an [**MapItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl) gebunden sind.

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

Die Eigenschaft ``ItemsSource`` im obigen Beispiel ist in der CodeBehind-Datei an eine Eigenschaft vom Typ [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist?view=netframework-4.70) gebunden.

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

## <a name="working-with-layers"></a>Arbeiten mit Schichten

In den Beispielen dieser Anleitung werden einer [MapElementLayers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer)-Sammlung Elemente hinzugefügt. Dann zeigen sie, wie diese Sammlung der Eigenschaft **Layers** des Kartensteuerelements hinzugefügt wird. In früheren Versionen wurde in dieser Anleitung gezeigt, wie Sie wie folgt der [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)-Sammlung Kartenelemente hinzufügen können:

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

Sie können diesen Ansatz weiterhin verwenden, doch werden Ihnen dann einige der Vorteile des neuen Kartenschichtenmodells nicht zur Verfügung stehen. Durch die Gruppierung der Elemente in Schichten können Sie jede davon unabhängig bearbeiten. Beispielsweise hat jede Ebene einen eigenen Satz an Ereignissen, sodass Sie auf ein Ereignis in einer bestimmten Ebene antworten und eine Aktion ausführen, die speziell für das Ereignis gilt.

Zudem können Sie XAML direkt an eine [MapLayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maplayer) binden. Das ist mit der [MapElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)-Sammlung nicht möglich.

Eine Möglichkeit besteht darin, eine Ansichtsmodellklasse, CodeBehind für eine XAML-Seite und eine XAML-Seite zu verwenden.

### <a name="view-model-class"></a>Ansichtsmodellklasse

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

### <a name="code-behind-a-xaml-page"></a>CodeBehind für eine XAML-Seite

Verknüpfen Sie die Ansichtsmodellklasse mit der CodeBehind-Seite.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>XAML-Seite

Binden Sie in der XAML-Seite die Eigenschaft aus der Ansichtsmodellklasse, die die Ebene zurückgibt.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>Verwandte Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Entwurfsrichtlinien für Karten](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)
* [**MapPolygon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)
* [**MapPolyline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline)
