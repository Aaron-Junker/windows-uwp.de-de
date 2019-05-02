---
description: Die Einträge und Eigenschaften eines Karten-Stylesheets
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Karten-Stylesheet-Referenz
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Karten, Karten-Stylesheet
ms.localizationpriority: medium
ms.openlocfilehash: 5f59775de8d86b5a0bae77d8c84e08e0328896f4
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816675"
---
# <a name="map-style-sheet-reference"></a>Karten-Stylesheet-Referenz

Microsoft Zuordnung-Technologien _zuordnen Stylesheets_ um die Darstellung von Zuordnungen definieren.  Ein Stylesheet für die Zuordnung wird mithilfe von JavaScript Object Notation (JSON) definiert und kann verwendet werden, in verschiedene Möglichkeiten, einschließlich in einer Windows Store-Anwendung [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) über die [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) -Methode.

Stylesheets können erstellt werden, interaktiv mit dem [Zuordnung Formatvorlagen-Editor](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) Anwendung.

Die folgende JSON-Code kann verwendet werden, um Wasser Bereiche in Rot angezeigt werden, Wasser Bezeichnungen in grün angezeigt und Land Bereiche, die in blau angezeigt werden:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Diese JSON kann verwendet werden, um alle Bezeichnungen und Punkte aus einer Zuordnung zu entfernen.

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

In einigen Fällen wird der Wert einer Eigenschaft transformiert, um das endgültige Ergebnis zu erzielen.  Beispielsweise hat die Pflanzen-FillColor je nach Typ der Entität geringfügig andere Töne.  Dieses Verhalten kann deaktiviert werden. So wird der exakte bereitgestellte Wert mithilfe der IgnoreTransform-Eigenschaft verwenden.

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

In diesem Thema werden die JSON-Einträge und [-Eigenschaften](#properties) behandelt, mit denen Sie das Aussehen und Verhalten von Karten anpassen können.  Diese Eigenschaften können auch angewendet werden, für Benutzer kartenelemente über die [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) Eigenschaft.

<a id="entries" />

## <a name="entries"></a>Einträge
In der folgenden Tabelle wird das Zeichen „>” verwendet, um Ebenen in der Eintragshierarchie darzustellen.  Es zeigt auch, welche Versionen von Windows jeder Eintrag unterstützen und die ignorieren.

| Version | Name der Windows-Version |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | Update April 2018    |
|  1809   | Update für Oktober 2018  |

| Name                         | Eigenschaftsgruppe            | 1703 | 1709 | 1803 | 1809 | Beschreibung    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | Die Stylesheet-Version, die Sie verwenden möchten |
| Einstellungen                     | [Einstellungen](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | Die Einstellungen, die für das gesamte Stylesheet gelten |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Der übergeordnete Eintrag für alle Karteneinträge |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Der übergeordnete Eintrag für alle Einträge mit Ausnahme von Benutzereinträgen |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Beschreiben Land Bereiche verwenden.  Diese sollte nicht zu verwechseln mit der physischen Gebäude unter dem Struktureintrag sind. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Flughäfen umfassen. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Bereiche, in denen es viele Unternehmen oder Sehenswürdigkeiten gibt |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die FRIEDHÖFEN umfassen. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Continent Bereich Bezeichnungen. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Schulen und andere Bildungseinrichtungen Funktionen umfassen. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die einheimischen Peoples umfassen reserviert. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Bereiche, die für gewerbliche Zwecke verwendet werden. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Insel Bereich Bezeichnungen. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die für medizinische Zwecke verwendet werden (z. B.: einen Campus Krankenhaus). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die militärische Basen umfassen oder militärische verwendet. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die nautische verwandten Zwecken verwendet werden. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bezeichnungen der Nachbarschaft-Bereich. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die als ein Flugzeug Weg verwendet wird. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Sandige Flächen wie Strände |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Flächen, die Einkaufspassagen oder Einkaufszentren zugeordnet sind |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die kuppeldach umfassen. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Unterirdische Bereiche (z. B. Fläche einer U-Bahn-Station) |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Wälder, Grünflächen usw. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Flächen mit Waldbestand |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Golf-Kurse umfasst. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Parks umfassen. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Als Spielfelder genutzte Flächen, wie Baseballfelder oder Tennisplätze |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Art umfassen reserviert. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Alle Point-Features, die mit einem Symbol für eine Art gezeichnet werden. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Zahlen Bezeichnungen zu beheben. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die natürliche Features darstellen. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Berggipfel darstellen |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Vulkangipfel darstellen |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Standorte von Wasseranlagen darstellen, wie Wasserfälle |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die alle interessanten Ort darstellen. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Die Symbole, die zur Darstellung der einem beliebigen Business-Speicherort. |
| >>>>> AttractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die Sport-Attraktionen z. B. Museen, Zoos usw. darstellen. |
| >>>>> CommunityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die zur Darstellung der Speicherorte der allgemeinen Verwendung für die Community. |
| >>>>> EducationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die zur Darstellung der Schulen und andere Bildungseinrichtungen im Zusammenhang mit Standorten. |
| >>>>> EntertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die Unterhaltung Veranstaltungsorten, z.B. Theater, Kinos usw. darstellen. |
| >>>>> EssentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die wichtige Dienste wie z.B. Parkmöglichkeiten sowie Banken, Gas darstellen. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die zur Darstellung der Restaurants, Cafés usw. |
| >>>>> LodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die Hotels und anderen Unternehmen Unterbringung darstellen. |
| >>>>> RealEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Die Symbole, die zur Darstellung der Immobilien-Unternehmen. |
| >>>>> ShoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Symbole, die Hotels und anderen Unternehmen Unterbringung darstellen. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Größe eines bewohnten Ortes darstellen (z. B. eine Stadt oder eine Ortschaft) |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Hauptstadt eines bewohnten Gebiets darstellen |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Hauptstadt von Bundesländern/Provinzen darstellen |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Hauptstadt von Ländern oder Regionen darstellen |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Zeichen, die die Bezeichnung einer Straße darstellen (Zum Beispiel: I-5). Verwenden Sie nur Palettenwerte, wenn Sie die **ImageFamily**-Eigenschaft des Einstellungseintrags auf einen Wert von *Palette* festlegen. |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Ausfahrten, in der Regel aus einer Autobahn mit Zugangsüberwachungssystem, darstellen |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Bushaltestellen, Zughaltestellen, Flughäfen usw. darstellen |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Politische Gebiete, wie Länder, Regionen und Staaten |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Country-Region Rahmen und Bezeichnungen. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, Staaten, Provinzen, usw., Ränder und Bezeichnungen. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, Landkreise, usw., Ränder und Bezeichnungen. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Häuser und andere gebäudeähnliche Bauten |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Gebäude. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Das Gebäude für Bildungseinrichtungen verwendet. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Das Gebäude für medizinische Zwecke, z. B. in Krankenhäusern verwendet. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Das Gebäude für die Übertragung wie Flughäfen verwendet. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linien, die Teil des Transportnetzwerks sind (z. B. Straßen, Züge und Fähren) |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linien, die alle Straßen darstellen |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die große, kontrollierten Zugriff Straßen darstellen. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linien, die Hochgeschwindigkeits-Rampen, die in der Regel darstellen mit verbinden gesteuert Zugriff Straßen. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Straßen darstellen. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die wichtige Straßen darstellen. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Arterieller Straßen darstellen. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Straßen darstellen. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Rampen darstellen, die in der Regel mit Straßen zu verbinden. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die unpaved Straßen darstellen. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Straßen, die Kosten Geld darstellen verwenden. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Eisenbahnlinien |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Spazierwege durch Parks oder Wanderwege |
| >>> Gehweg eingetragen                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Mit erhöhten Rechten Gehweg eingetragen. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Fährlinien |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Alle Elemente, die wie Wasser aussehen. Dazu zählen Ozeane und Flüsse. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Flüsse, Ströme oder andere Wasserstraßen.  Beachten Sie, dass es sich herbei um Linien oder Polygone handeln kann, die zu stehenden Gewässern verbinden können. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Alle Weiterleitung verwandten Einträgen. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Weiterleiten von Line-bezogenen Einträge. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die treibende Routen darstellen. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zeilen, die scenic treibende Routen darstellen. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Positionen darstellen, die Routen zu durchlaufen. |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Benutzereinträge für alle. |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)-Instanzen |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline)-Instanzen |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d)-Instanzen.  Dies gilt in erster Linie zur Einstellung von renderAsSurface. |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon)-Instanzen |

<a id="properties" />

## <a name="properties"></a>Eigenschaften

In diesem Abschnitt werden die Eigenschaften beschrieben, die Sie für die Einträge verwenden können.

<a id="version" />

### <a name="version-properties"></a>Versionseigenschaften

| Eigenschaft                     | Typ    | Beschreibung                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | Zeichenfolge  | Version des zu verwendenden Stylesheets. Wird für die Anwendbarkeit verwendet. „1.0” als Standard, „1. *” für zusätzliche geringfügige Featureupdates |

<a id="settings" />

### <a name="settings-properties"></a>Einstellungseigenschaften

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | Beschreibung |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob die Atmosphäre im 3D-Steuerelement angezeigt wird |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   | Ein Flag, das angibt, ob Texturen auf symbolischen 3D-Gebäuden, die über Texturen verfügen, angezeigt werden sollen oder nicht |
| fogColor                     | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert des Distanznebels, der im 3D-Steuerelement angezeigt wird |
| glowColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert, der für glänzende Beschriftungen und Symbole angewendet werden kann |
| imageFamily                  | Zeichenfolge  |  ✔   |  ✔   |  ✔   |  ✔   | Der Name der Bildzusammenstellung, die für dieses Format verwendet werden soll. Legen Sie diesen Wert auf *Default* für Zeichen fest, die feste Farben verwenden, die auf den Zeichen in der realen Welt basieren. Legen Sie diesen Wert auf *Palette* für Zeichen fest, die über die Palette konfigurierbare Farben verwenden. |
| landColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert von Landflächen, bevor etwas darauf gezeichnet wird |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Elemente mit einer **Organization**-Eigenschaft die entsprechenden Logos zeichnen oder ein allgemeines Symbol verwenden sollen |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Elemente, die über eine offizielle Farbeigenschaft verfügen (wie Transitlinien in China), in dieser Farbe gezeichnet werden sollen. Deaktivieren Sie diesen Wert beispielsweise für Schwarz-Weiß-Karten. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, der angibt, ob Raster Regionen gezeichnet werden soll, in dem sie eine bessere Darstellung als Vektoren (Japan und Südkorea) verfügen. |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Schattierungen für Erhöhungen auf der Karte gezeichnet werden sollen |
| shadedReliefDarkColor        | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe der dunklen Seite des schattierten Reliefs.  Alpha-Kanal stellt die maximale alpha-Wert dar. |
| shadedReliefLightColor       | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe der hellen Seite des schattierten Reliefs.  Alpha-Kanal stellt die maximale alpha-Wert dar. |
| shadowColor                  | Farbe   |      |      |      |  ✔   | Die Farbe des Schattens hinter Symbole, die Schatten zu verwenden. |
| spaceColor                   | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert für den Bereich um die Karte herum |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob die ursprünglichen Farben in das SVG-Format verwendet, anstatt den Palette-Eintrag für die Farben in einem Image suchen werden soll. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement-Eigenschaften

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | Beschreibung |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   | Betrag, um den das Hintergrundelement eines Symbols skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| fillColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe, die für das Füllen von Polygonen, den Hintergrund von Punktsymbolen und für die Mitte von geteilten Linien verwendet wird |
| fontFamily                   | Zeichenfolge  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe der Glyphe, die in der Mitte eines Punktsymbols angezeigt wird |
| iconScale                    | Gleitkomma   |      |  ✔   |  ✔   |  ✔   | Betrag, um den die Glyphe eines Symbols skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| labelColor                   | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   | Der Wert, um den die Standardgröße von Beschriftungen skaliert wird. Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Führt das Überschreiben von **StrokeColor** mit dem Alphawert von **FillColor** aus, anstatt eine Mischung der beiden Farben zu verwenden |
| scale                        | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   | Der Wert, um den die gesamte Größe des Punkts skaliert wird. Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| strokeColor                  | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe, die für die Kontur von Polygonen, die Kontur von Punktsymbolen und die Farbe von Linien verwendet werden soll |
| strokeWidthScale             | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   | Der Wert, um den die Strichstärke von Linien skaliert wird Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Diese Eigenschaftengruppe erbt von der [MapElement](#mapelement)-Eigenschaftengruppe.

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | Beschreibung |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die sekundäre oder Umrandungslinienfarbe des Rahmens eines gefüllten Polygons |
| borderStrokeColor            | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die primäre Farbe des Rahmens eines gefüllten Polygons |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   | Der Betrag, um der Strich der Rahmenlinien, die skaliert werden. Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle-Eigenschaften

Diese Eigenschaftengruppe erbt von der [MapElement](#mapelement)-Eigenschaftengruppe.

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | Beschreibung |
|------------------------------|---------|------|------|------|------|-------------|
| Shape-Hintergrund             | Gleitkomma   |      |      |      |  ✔   | Form, der als Hintergrund des Symbols – ersetzen jede Form, die vorhanden ist. |
| stemAnchorRadiusScale        | Gleitkomma   |      |      |  ✔   |  ✔   | Betrag, um den der Ankerpunkt eines Symbolschafts skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| stemColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe des Schafts unten an einem Symbol im 3D-Modus |
| stemHeightScale              | Gleitkomma   |      |      |  ✔   |  ✔   | Betrag, um den die Länge eines Symbolschafts skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Länge. |
| stemOutlineColor             | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe der Kontur um den Schaft unten an einem Symbol im 3D-Modus |
| stemWidthScale               | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   | Betrag, um den die Breite eines Symbolschafts skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Länge. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Diese Eigenschaftengruppe erbt von der [MapElement](#mapelement)-Eigenschaftengruppe.

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | Beschreibung |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | Ein Flag, das angibt, dass ein 3D-Modell wie ein Gebäude gerendert werden soll – ohne Tiefenausblendung gegen den Boden |
