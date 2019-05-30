---
title: Überlagern von nebeneinander angeordneten Bildern in einer Karte
description: Überlagern Sie Bilder von Drittanbietern oder benutzerdefinierte nebeneinander angeordnete Bilder in einer Karte mithilfe von Kachelquellen. Verwenden Sie Kachelquellen, um spezielle Infos wie Wetterdaten, Einwohnerzahlen oder seismische Daten zu überlagern oder die Standardkarte vollständig zu ersetzen.
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.date: 07/19/2018
ms.topic: article
keywords: Windows 10, UWP, Karte, Standort, Bilder, Überlagerung
ms.localizationpriority: medium
ms.openlocfilehash: e9b4d439958e6cfbf0845aaf5bcd31644ff39432
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371684"
---
# <a name="overlay-tiled-images-on-a-map"></a>Überlagern von nebeneinander angeordneten Bildern in einer Karte

Überlagern Sie Bilder von Drittanbietern oder benutzerdefinierte nebeneinander angeordnete Bilder in einer Karte mithilfe von Kachelquellen. Verwenden Sie Kachelquellen, um spezielle Infos wie Wetterdaten, Einwohnerzahlen oder seismische Daten zu überlagern oder die Standardkarte vollständig zu ersetzen.

**Tipp**: Laden Sie das [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619977) auf Github herunter, um mehr über die Verwendung von Karten in Ihrer App zu erfahren.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>Übersicht über nebeneinander angeordnete Bilder

Kartendienste wie Nokia Karten und Bing Maps teilen Karten zum schnellen Abrufen und Anzeigen in quadratische Kacheln ein. Diese Kacheln messen 256 x 256 Pixel und werden vorab auf mehrere Detailebenen gerendert. Viele Dienste von Drittanbietern umfassen auch kartenbasierte Daten, die in Kacheln aufgeteilt sind. Verwenden Sie Kachelquellen, um Kacheln von Drittanbietern abzurufen oder benutzerdefinierte Kacheln zu erstellen. Überlagern Sie diese auf der angezeigten Karte im [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Objekt.

**Wichtige**    , wenn Sie die Kachel-Datenquellen verwenden, müssen Sie nicht Schreiben von Code, um anzufordern, oder um einzelne Kacheln zu positionieren. Das [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Objekt fordert bei Bedarf Kacheln an. Bei jeder Anforderung werden die X- und Y-Koordinaten und der Zoomfaktor für die einzelnen Kacheln angegeben. Geben Sie einfach das Format des URIs oder Dateinamens an, um die Kacheln in der **UriFormatString**-Eigenschaft abzurufen. Dazu fügen Sie ersetzbare Parameter in den Basis-URI oder Dateinamen ein, um anzugeben, wo die X- und Y-Koordinaten und der Zoomfaktor für jede Kachel übergeben werden.

In diesem Beispiel sehen Sie eine [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft für eine [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)-Klasse. Diese zeigt die ersetzbaren Parameter für die X- und Y-Koordinaten und den Zoomfaktor an.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

(Die X- und Y-Koordinaten stellen die Position der einzelnen Kachel in der Zuordnung der Weltkarte auf der angegebenen Detailebene dar. Die Nummerierung der Kachel beginnt bei {0, 0} in der oberen linken Ecke der Karte. Die Kachel bei {1, 2} befindet sich beispielsweise in der zweiten Spalte der dritten Zeile des Kachelrasters.)

Weitere Informationen über das Kachelsystem von Kartendiensten finden Sie unter [Kachelsystem von Bing Maps](https://go.microsoft.com/fwlink/p/?LinkId=626692).

### <a name="overlay-tiles-from-a-tile-source"></a>Überlagern von Kacheln aus einer Kachelquelle

Überlagern Sie nebeneinander angeordnete Bilder aus einer Kachelquelle in einer Karte mithilfe der [**MapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource)-Klasse.

1.  Instanziieren Sie eine der drei Klassen für Kacheldatenquellen, die von der [**MapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource)-Klasse erben.

    -   [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)
    -   [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)
    -   [**CustomMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)

    Konfigurieren Sie die **UriFormatString**-Eigenschaft zum Anfordern der Kacheln, indem Sie ersetzbare Parameter im Basis-URI oder Dateinamen einfügen.

    Im folgenden Beispiel wird eine [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)-Klasse instanziiert. In diesem Beispiel wird der Wert der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft im Konstruktor der **HttpMapTileDataSource**-Klasse veranschaulicht.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  Instanziieren und konfigurieren Sie eine [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)-Klasse. Geben Sie die [**MapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource)-Klasse an, die Sie im vorherigen Schritt als [**DataSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource)-Eigenschaft der **MapTileSource**-Klasse festgelegt haben.

    Im folgenden Beispiel wird die [**DataSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource)-Eigenschaft im Konstruktor der [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)-Klasse veranschaulicht.

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    Sie können die Bedingungen, unter denen Kacheln angezeigt werden, mithilfe der Eigenschaften der [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)-Klasse einschränken.

    -   Zeigen Sie Kacheln nur in einem bestimmten geografischen Bereich an, indem Sie einen Wert für die [**Bounds**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds)-Eigenschaft angeben.
    -   Zeigen Sie Kacheln nur auf bestimmten Detailebenen an, indem Sie einen Wert für die [**ZoomLevelRange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange)-Eigenschaft angeben.

    Optional können Sie weitere Eigenschaften der [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)-Klasse konfigurieren, die Auswirkungen auf das Laden oder die Anzeige von Kacheln haben, z. B. [**Layer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer), [**AllowOverstretch**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.allowoverstretch), [**IsRetryEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.isretryenabled) und [**IsTransparencyEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.istransparencyenabled).

3.  Fügen Sie die [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)-Klasse der [**TileSources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.tilesources)-Collection dem [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Objekt hinzu.

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>Überlagern von Kacheln aus einem Webdienst


Überlagern Sie nebeneinander angeordnete Bilder aus einem Webdienst mithilfe der [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)-Klasse.

1.  Instanziieren Sie eine [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)-Klasse.
2.  Geben Sie das Format des URIs an, den der Webdienst als Wert für die [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft erwartet. Um diesen Wert zu erstellen, fügen Sie dem Basis-URI die ersetzbaren Parameter hinzu. Im folgenden Codebeispiel beträgt der Wert der **UriFormatString**-Eigenschaft beispielsweise:

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    Der Webdienst muss einen URI unterstützen, der die ersetzbaren Parameter {x}, {y} und {zoomlevel} enthält. Die meisten Webdienste (z. B. Nokia, Bing und Google) unterstützen URIs in diesem Format. Benötigt der Webdienst zusätzliche Argumente, die mit der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft nicht zur Verfügung stehen, müssen Sie einen benutzerdefinierten URI erstellen. Mithilfe des [**UriRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.urirequested)-Ereignisses können Sie einen benutzerdefinierten URI erstellen und zurückgeben. Weitere Infos finden Sie im Abschnitt [Bereitstellen eines benutzerdefinierten URIs](#customuri) weiter unten in diesem Thema.

3.  Befolgen Sie die verbleibenden Schritte in der [Übersicht über nebeneinander angeordnete Bilder](#tileintro).

Im folgenden Beispiel überlagern Kacheln aus einem fiktiven Webdienst eine Karte von Nordamerika. Der Wert der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft ist im Konstruktor der [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)-Klasse angegeben. In diesem Beispiel werden durch Festlegen der optionalen [**Bounds**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds)-Eigenschaft nur Kacheln innerhalb der geografischen Bereiche angegeben.

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>Überlagern von Kacheln aus dem lokalen Speicher


Überlagern Sie nebeneinander angeordnete Bilder, die als Dateien im lokalen Speicher gespeichert sind, mithilfe der [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)-Klasse. In der Regel können Sie diese Dateien mit Ihrer App verpacken und verteilen.

1.  Instanziieren Sie eine [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)-Klasse.
2.  Geben Sie das Format der Dateinamen als Wert für die [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring)-Eigenschaft ein. Um diesen Wert zu erstellen, fügen Sie dem Basis-Dateinamen die ersetzbaren Parameter hinzu. Im folgenden Codebeispiel hat die [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft beispielsweise den Wert:

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    Wenn das Format der Dateinamen zusätzliche Argumente benötigt, die mit der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring)-Eigenschaft nicht zur Verfügung stehen, müssen Sie einen benutzerdefinierten URI erstellen. Mithilfe des [**UriRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.urirequested)-Ereignisses können Sie einen benutzerdefinierten URI erstellen und zurückgeben. Weitere Infos finden Sie im Abschnitt [Bereitstellen eines benutzerdefinierten URIs](#customuri) weiter unten in diesem Thema.

3.  Befolgen Sie die verbleibenden Schritte in der [Übersicht über nebeneinander angeordnete Bilder](#tileintro).

Zum Laden von Kacheln aus dem lokalen Speicher können Sie die folgenden Protokolle und Speicherorte verwenden:

| Uri | Weitere Informationen |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | Verweist auf das Stammelement des App-Installationsordners. |
|  | Dies ist der von der [Package.InstalledLocation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.installedlocation)-Eigenschaft referenzierte Speicherort. |
| ms-appdata:///local | Verweist auf das Stammelement des lokalen Speichers der App. |
|  | Dies ist der von der [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)-Eigenschaft referenzierte Speicherort. |
| ms-appdata:///temp | Verweist auf die temporären Ordner der App. |
|  | Dies ist der Pfad, auf den die [ApplicationData.TemporaryFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)-Eigenschaft verweist. |

 

Im folgenden Beispiel werden Kacheln, die als Dateien im Installationsverzeichnis der App gespeichert werden, mithilfe des `ms-appx:///`-Protokolls geladen. Der Wert der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring)-Eigenschaft ist im Konstruktor der [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)-Klasse angegeben. In diesem Beispiel werden Kacheln nur angezeigt, wenn der Zoomfaktor für die Karte innerhalb des Bereichs liegt, der durch die optionale [**ZoomLevelRange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange)-Eigenschaft angegeben ist.

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>Bereitstellen eines benutzerdefinierten URIs

Wenn die ersetzbaren Parameter, die mit der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)-Eigenschaft der [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)-Klasse oder der [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring)-Eigenschaft der [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)-Klasse zur Verfügung stehen, nicht zum Abrufen Ihrer Kacheln ausreichen, müssen Sie einen benutzerdefinierten URI erstellen. Indem Sie einen benutzerdefinierten Handler für das **UriRequested**-Ereignis bereitstellen, können Sie einen benutzerdefinierten URI erstellen und zurückgeben. Das **UriRequested**-Ereignis wird für jede einzelne Kachel ausgelöst.

1.  Kombinieren Sie in Ihrem benutzerdefinierten Handler für das **UriRequested**-Ereignis die erforderlichen benutzerdefinierten Argumente mit den Eigenschaften [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.x), [**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.y) und [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.zoomlevel) der [**MapTileUriRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs)-Klasse, um den benutzerdefinierten URI zu erstellen.
2.  Geben Sie die benutzerdefinierte URI in der [**Uri**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequest.uri)-Eigenschaft der [**MapTileUriRequest**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequest)-Klasse zurück. Diese ist in der [**Request**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.request)-Eigenschaft der [**MapTileUriRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs)-Klasse enthalten.

Das folgende Beispiel zeigt, wie Sie einen benutzerdefinierten URI durch Erstellen eines benutzerdefinierten Handlers für das **UriRequested**-Ereignis bereitstellen. Sie erfahren zudem, wie Sie das Verzögerungsmuster implementieren, wenn Sie asynchron zum Erstellen des benutzerdefinierten URIs weitere Schritte ausführen müssen.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>Überlagern von Kacheln aus einer benutzerdefinierten Quelle

Überlagern Sie benutzerdefinierte Kacheln mithilfe der [**CustomMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)-Klasse. Erstellen Sie Kacheln programmgesteuert im Arbeitsspeicher, oder schreiben Sie eigenen Code, um vorhandene Kacheln aus einer anderen Quelle zu laden.

Stellen Sie zum Erstellen oder Laden von benutzerdefinierten Kacheln einen benutzerdefinierten Handler für das [**BitmapRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested)-Ereignis bereit. Das **BitmapRequested**-Ereignis wird für jede einzelne Kachel ausgelöst.

1.  Kombinieren Sie in Ihrem benutzerdefinierten Handler für das [**BitmapRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested)-Ereignis die erforderlichen benutzerdefinierten Argumente mit den Eigenschaften [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) und [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) der [**MapTileBitmapRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs)-Klasse, um eine benutzerdefinierte Kachel zu erstellen oder abzurufen.
2.  Geben Sie die benutzerdefinierte Kachel in der [**PixelData**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequest.pixeldata)-Eigenschaft der [**MapTileBitmapRequest**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequest)-Klasse zurück, die in der [**Request**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.request)-Eigenschaft der [**MapTileBitmapRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs)-Klasse enthalten ist. Bei der **PixelData**-Eigenschaft handelt es sich um den Typ [**IRandomAccessStreamReference**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStreamReference).

Das folgende Beispiel zeigt, wie Sie benutzerdefinierte Kacheln durch Erstellen eines benutzerdefinierten Handlers für das **BitmapRequested**-Ereignis bereitstellen. In diesem Beispiel werden identische rote Kacheln erstellt, die teilweise deckend sind. Im Beispiel werden die Eigenschaften [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) und [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) der [**MapTileBitmapRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs)-Klasse ignoriert. Auch wenn dies kein praktisches Beispiel ist, wird im Beispiel veranschaulicht, wie Sie benutzerdefinierte Kacheln im Arbeitsspeicher erstellen können. Sie erfahren zudem, wie Sie das Verzögerungsmuster implementieren, wenn Sie asynchron zum Erstellen der benutzerdefinierten Kacheln weitere Schritte ausführen müssen.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>Ersetzen der Standardkarte

So ersetzen Sie die Standardkarte durch Drittanbieter- oder benutzerdefinierte Kacheln:

-   Legen Sie [**MapTileLayer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileLayer).**BackgroundReplacement** als Wert für die [**Layer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer)-Eigenschaft der [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)-Klasse fest.
-   Legen Sie [**MapStyle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle).**None** als Wert für die [**Style**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style)-Eigenschaft der [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse fest.

## <a name="related-topics"></a>Verwandte Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Entwurfsrichtlinien für Karten](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://go.microsoft.com/fwlink/p/?LinkId=619982)
