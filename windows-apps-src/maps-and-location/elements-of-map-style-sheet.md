---
description: Die Einträge und Eigenschaften eines Karten-Stylesheets
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Karten-Stylesheet-Referenz
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Karten, Karten-Stylesheet
ms.localizationpriority: medium
ms.openlocfilehash: b2e6e57721a5667a9ca38b21eee2a618353cd30b
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218560"
---
# <a name="map-style-sheet-reference"></a>Karten-Stylesheet-Referenz

Microsoft-Zuordnungs Technologien verwenden _Karten Stylesheets_ , um die Darstellung von Zuordnungen zu definieren.  Ein Karten Stylesheet wird mithilfe von JavaScript Object Notation (JSON) definiert und kann auf verschiedene Weise verwendet werden, einschließlich der [mapcontrol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) -Methode von Windows Store-Anwendungen über die [mapstylesheet. Parser-fromjson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) -Methode.

Stylesheets können interaktiv mithilfe der Editor-Anwendung des [Karten Stylesheets](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) erstellt werden.

Der folgende JSON-Code kann verwendet werden, um Wasser Bereiche in rot anzuzeigen, Wasser Bezeichnungen werden grün angezeigt, und Länder Bereiche werden in blau angezeigt:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Dieser JSON-Code kann verwendet werden, um alle Bezeichnungen und Punkte aus einer Karte zu entfernen.

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

In diesem Thema werden die JSON-Einträge und [-Eigenschaften](#properties) behandelt, mit denen Sie das Aussehen und Verhalten von Karten anpassen können.  Diese Eigenschaften können auch auf Benutzer Zuordnungs Elemente über die [mapstylesheetentry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) -Eigenschaft angewendet werden.

<a id="entries" />

## <a name="entries"></a>Einträge
In der folgenden Tabelle wird das Zeichen „>” verwendet, um Ebenen in der Eintragshierarchie darzustellen.  Außerdem wird gezeigt, welche Versionen von Windows die einzelnen Einträge unterstützen und welche ignoriert werden.

| Version | Windows-Releasename |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | Update April 2018    |
|  1809   | Update vom Oktober 2018  |
|  1903   | Update vom Mai 2019      |

| Name                               | Eigenschaftsgruppe            | 1703 | 1709 | 1803 | 1809 | 1903 | Beschreibung    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| version                            | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Stylesheet-Version, die Sie verwenden möchten |
| Einstellungen                           | [Einstellungen](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Einstellungen, die für das gesamte Stylesheet gelten |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der übergeordnete Eintrag für alle Karteneinträge |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der übergeordnete Eintrag für alle Einträge mit Ausnahme von Benutzereinträgen |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die den Land Gebrauch beschreiben.  Diese sollten nicht mit den physischen Gebäuden in dem Struktur Eintrag verwechselt werden. |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Flughäfen umfassen. |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, in denen es viele Unternehmen oder Sehenswürdigkeiten gibt |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Friedhöfe umfassen. |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Kontinente Flächen Bezeichnungen. |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Schulen und andere Bildungseinrichtungen umfassen. |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die indigene Völker abdecken, sind reserviert. |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die für Industrie Zwecke verwendet werden. |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Insel Flächen Bezeichnungen. |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die für medizinische Zwecke verwendet werden (z. b. ein krankenhauscampus). |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die militärische Basen umfassen oder militärisch verwendet werden. |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die für die zu einem Gebiet Gehör Ende Zwecke verwendet werden. |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Raum Bezeichnungen für die Region. |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die als Flugzeug-Landebahn verwendet werden. |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Sandige Flächen wie Strände |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Flächen, die Einkaufspassagen oder Einkaufszentren zugeordnet sind |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Stadien umfassen. |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Unterirdische Bereiche (z. B. Fläche einer U-Bahn-Station) |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Wälder, Grünflächen usw. |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Flächen mit Waldbestand |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Golf Kurse umfassen. |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Park Bereiche umfassen. |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Als Spielfelder genutzte Flächen, wie Baseballfelder oder Tennisplätze |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bereiche, die Natur Reserven umfassen. |
| > > frozenwater                     | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Fixierter Wasser, wie Gletscher. |
| >> point                           | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Alle Point-Funktionen, die mit einem Symbol einiger Art gezeichnet werden. |
| >>> address                        | [Pointstyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | Adress Nummern Bezeichnungen. |
| >>> naturalPoint                   | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die natürliche Funktionen darstellen. |
| >>>> peak                          | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Berggipfel darstellen |
| >>>>> volcanicPeak                 | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Vulkangipfel darstellen |
| >>>> waterPoint                    | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Standorte von Wasseranlagen darstellen, wie Wasserfälle |
| >>> pointOfInterest                | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die einen beliebigen interessanten Speicherort darstellen. |
| >>>> business                      | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die alle Geschäfts Orte darstellen. |
| > > > > > attractionpoint              | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die touristische Attraktionen darstellen, wie z. b. Museen, Zoos usw. |
| > > > > > > "amusementplacepoint"         | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für den vergnüsort. |
| > > > > > >.               | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das Aquarien Symbol. |
| > > > > > > artgallerypoint             | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für Kunstgalerie. |
| > > > > > > Mobile Point                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Camp-Symbol. |
| > > > > > > fishingpoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Angel Symbol. |
| > > > > > > Gärtnerei Punkt                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Garten Symbol. |
| > > > > > > hikingpoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Wander Symbol. |
| > > > > > > librarypoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Bibliotheks Symbol. |
| > > > > > > museumpoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Museum". |
| > > > > > > naturalplacepoint           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für natürliches platzieren. |
| > > > > > >.                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Park Symbol. |
| > > > > > > restareapoint               | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für den Rest-Bereich. |
| > > > > > >.      | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für Touristenattraktion. |
| > > > > > > Zoomen                    | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Zoosymbol. |
| > > > > > communitypoint               | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Orte der allgemeinen Verwendung für die Community darstellen. |
| > > > > > > konventionencenterpoint       | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für das Konvention-Center. |
| > > > > > > "financialpoint"              | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Finanz Symbol. |
| > > > > > > governmentpoint             | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Regierung". |
| > > > > > > informationtechnologypoint  | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Informationstechnologie". |
| > > > > > > palacepoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Schloss Symbol. |
| > > > > > > pollingstationpoint         | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Abruf Station". |
| > > > > > > Research Point               | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Forschungs Symbol. |
| > > > > > educationpoint               | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Schulen und andere Bildungseinrichtungen darstellen. |
| > > > > > entertainmentpoint           | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Unterhaltungsveranstaltungen darstellen, wie z. b. Theater, Kinos usw. |
| > > > > > > artspoint                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Kunst" |
| > > > > > > bowlingpoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Bowlingsymbol. |
| > > > > > > casinopoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das Casino-Symbol. |
| > > > > > >.             | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für den Golf Kurs. |
| > > > > > >-Sicherungspunkt                    | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Fitness". |
| > > > > > >.                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Marina". |
| > > > > > >.           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Film Theater Symbol. |
| > > > > > > Nightclub Point              | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Nacht Club Symbol. |
| > > > > > > "neu erstellen"             | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für die Wiederholung. |
| > > > > > >-skatingpoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Schlittschuh Symbol. |
| > > > > > > skiareapoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Ski Area Icon. |
| > > > > > >.                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das poolpoolsymbol. |
| > > > > > > poolpoint           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das poolpoolsymbol. |
| > > > > > > Theater Punkt                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Theater Symbol. |
| > > > > > > winerypoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Winery-Symbol. |
| > > > > > essentialservicepoint        | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die wichtige Dienste darstellen, wie z. b. Parkplätze, Banken, Gas usw. |
| > > > > > > atmpoint                    | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | ATM-Symbol. |
| > > > > > >.       | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Auto Mobil Verleih-Symbol. |
| > > > > > > Auto Report Point       | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Auto Mobil Reparatur Symbol. |
| > > > > > > bankpoint                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Bank Symbol. |
| > > > > > > clinicpoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Klinikum (Symbol). |
| > > > > > > "elektricchargingstationpoint"| [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "elektrische Ladestation". |
| > > > > > > firestationpoint            | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Feuer Station-Symbol. |
| > > > > > >.             | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das erstellungensymbol. |
| > > > > > > grocerypoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Lebensmittel Symbol. |
| > > > > > > hospitalpoint               | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Krankenhaus Symbol. |
| > > > > > > launtrockenpunkt                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Wäscherei". |
| > > > > > > "liquorandwinestorepoint"     | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Spirituosen und Wein Speicher". |
| > > > > >-Postpoint                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Mail". |
| > > > > > > marketpoint                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für den Markt. |
| > > > > > >.                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Einstellungensymbol |
| > > > > > > petspoint                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Haustiere (Symbol). |
| > > > > > >-pharmaypoint               | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Apotheken Symbol. |
| > > > > > > Richtlinien Punkt                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Polizei Symbol. |
| > > > > > > postalservicepoint          | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Postservice-Symbol. |
| > > > > > > professionalpoint           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Professional Service". |
| > > > > > > toiletpunkt                 | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das WC-Symbol. |
| > > > > >-veterinarianpoint           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Tierarzt-Symbol. |
| >>>>> foodPoint                    | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Restaurants, Cafés usw. darstellen |
| > > > > > > barandgrillpoint            | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für Balken und Grillen. |
| > > > > > > barpunkt                    | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Balken Symbol. |
| > > > > > >-Brauerei Punkt                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Brauerei Symbol. |
| > > > > > >-Punkt                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Icon-Symbol. |
| > > > > > > icecreamshoppoint           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für eissahne-Geschäft. |
| > > > > > >-restaurantpoint             | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Restaurant Symbol. |
| > > > > > > teashoppoint                | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Teashop-Symbol. |
| > > > > > lodgingpoint                 | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Hotels und andere Unternehmen darstellen. |
| > > > > > > gotelpoint                  | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Hotel Symbol. |
| > > > > > realestatepoint              | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Immobilien Unternehmen darstellen. |
| > > > > > shoppingpoint                | [Pointstyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Hotels und andere Unternehmen darstellen. |
| > > > > > > "Automobilhändler Point"       | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für den Automobilhändler. |
| > > > > > > beautyandspapoint           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Schönheit und Spa". |
| > > > > > > bookstorepoint              | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Buchbuchbuch. |
| > > > > > > departmentstorepoint        | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Abteilungs speichersymbol. |
| > > > > > > "        | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Das Symbol für den Elektronik Shop. |
| > > > > > > golfshoppoint               | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol für den Golf Shop. |
| > > > > > > homeapplianceshoppoint      | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Start-Appliance-Symbol. |
| > > > > > > mallpoint                   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbol "Mall". |
| > > > > > "phoneshoppoint"              | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Telefon Shop-Symbol. |
| >>> populatedPlace                 | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Größe eines bewohnten Ortes darstellen (z. B. eine Stadt oder eine Ortschaft) |
| >>>> capital                       | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Hauptstadt eines bewohnten Gebiets darstellen |
| >>>>> adminDistrictCapital         | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Hauptstadt von Bundesländern/Provinzen darstellen |
| > > > > > > majoradmindistrictcapital   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbole, die die Hauptstadt eines Bundesstaats oder Provinz darstellen. |
| > > > > > > minoradmindistrictcapital   | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbole, die das neben-Haupt Kapital eines Bundesstaats oder Provinz darstellen. |
| >>>>> countryRegionCapital         | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die die Hauptstadt von Ländern oder Regionen darstellen |
| > > > > majorpopulatedplace           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbole, die die Größe des großen aufgefüllten Platzes darstellen. |
| > > > > minorpopulatedplace           | [Pointstyle](#pointstyle) |      |      |      |      |  ✔   | Symbole, die die Größe des geringfügigen aufgefüllten Platzes darstellen. |
| >>> roadShield                     | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeichen, die die Bezeichnung einer Straße darstellen (zum Beispiel: I-5). Verwenden Sie nur Palettenwerte, wenn Sie die **ImageFamily**-Eigenschaft des Einstellungseintrags auf einen Wert von *Palette* festlegen. |
| >>> roadExit                       | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Ausfahrten, in der Regel aus einer Autobahn mit Zugangsüberwachungssystem, darstellen |
| >>> transit                        | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Symbole, die Bushaltestellen, Zughaltestellen, Flughäfen usw. darstellen |
| >> political                       | [Borderedmapelement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Politische Gebiete, wie Länder, Regionen und Staaten |
| >>> countryRegion                  | [Borderedmapelement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Länder Regions Grenzen und-Bezeichnungen. |
| >>> adminDistrict                  | [Borderedmapelement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, States, Provinzen usw., Rahmen und Bezeichnungen. |
| >>> district                       | [Borderedmapelement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, Countys usw., Rahmen und Bezeichnungen. |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Häuser und andere gebäudeähnliche Bauten |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bau. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Für Bildungseinrichtungen verwendete Gebäude. |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Gebäude, die für medizinische Zwecke wie Krankenhäuser verwendet werden. |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Für die Übertragung verwendete Gebäude, z. b. Flughäfen |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linien, die Teil des Transportnetzwerks sind (z. B. Straßen, Züge und Fähren) |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linien, die alle Straßen darstellen |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die große, gesteuerte Zugriffs Straßen darstellen. |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die hoch Geschwindigkeits Rampen darstellen, die in der Regel mit kontrollierten Zugriffs Straßen verbunden werden. |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Straßen darstellen. |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linien, die Hauptstraßen darstellen. |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die auf dem Weg arterielle Straßen darstellen. |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Straßen darstellen. |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Rampen darstellen, die in der Regel eine Verbindung zu den- |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die nicht gepflasterte Straßen darstellen. |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Straßen darstellen, die Geld Kosten verursachen. |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Eisenbahnlinien |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Spazierwege durch Parks oder Wanderwege |
| > > >-Gang                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Erhöhter Gang. |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Fährlinien |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Alle Elemente, die wie Wasser aussehen. Dazu zählen Ozeane und Flüsse. |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Flüsse, Ströme oder andere Wasserstraßen.  Beachten Sie, dass es sich herbei um Linien oder Polygone handeln kann, die zu stehenden Gewässern verbinden können. |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Alle Routing bezogenen Einträge. |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen bezogene Einträge weiterleiten. |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Fahr Routen darstellen. |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die die Routen für das Routing darstellen. |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zeilen, die Wanderrouten darstellen. |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Alle Benutzereinträge. |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)-Instanzen |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline)-Instanzen |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d)-Instanzen.  Dies gilt in erster Linie zur Einstellung von renderAsSurface. |
| >> userPoint                       | [Pointstyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Das Format für Standard-[MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon)-Instanzen |

<a id="properties" />

## <a name="properties"></a>Eigenschaften

In diesem Abschnitt werden die Eigenschaften beschrieben, die Sie für die Einträge verwenden können.

<a id="version" />

### <a name="version-properties"></a>Versionseigenschaften

| Eigenschaft                     | Typ    | Beschreibung                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | Version des zu verwendenden Stylesheets. Wird für die Anwendbarkeit verwendet. „1.0” als Standard, „1. *” für zusätzliche geringfügige Featureupdates |

<a id="settings" />

### <a name="settings-properties"></a>Einstellungseigenschaften

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | 1903 | Beschreibung |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob die Atmosphäre im 3D-Steuerelement angezeigt wird |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Texturen auf symbolischen 3D-Gebäuden, die über Texturen verfügen, angezeigt werden sollen oder nicht |
| fogColor                     | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert des Distanznebels, der im 3D-Steuerelement angezeigt wird |
| glowColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert, der für glänzende Beschriftungen und Symbole angewendet werden kann |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der Name der Bildzusammenstellung, die für dieses Format verwendet werden soll. Legen Sie diesen Wert auf *Default* für Zeichen fest, die feste Farben verwenden, die auf den Zeichen in der realen Welt basieren. Legen Sie diesen Wert auf *Palette* für Zeichen fest, die über die Palette konfigurierbare Farben verwenden. |
| landColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert von Landflächen, bevor etwas darauf gezeichnet wird |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Elemente mit einer **Organization**-Eigenschaft die entsprechenden Logos zeichnen oder ein allgemeines Symbol verwenden sollen |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Elemente, die über eine offizielle Farbeigenschaft verfügen (wie Transitlinien in China), in dieser Farbe gezeichnet werden sollen. Deaktivieren Sie diesen Wert beispielsweise für Schwarz-Weiß-Karten. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Raster Bereiche gezeichnet werden sollen, in denen Sie über eine bessere Darstellung als Vektoren (Japan und Korea) verfügen. |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob Schattierungen für Erhöhungen auf der Karte gezeichnet werden sollen |
| ShadowColor                  | Farbe   |      |      |      |  ✔   |  ✔   | Die Farbe des Schattens hinter Symbolen, die Schatten verwenden. |
| spaceColor                   | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der ARGB-Farbwert für den Bereich um die Karte herum |
| terrainflat                  | Bool    |      |      |      |      |      | Ein Flag, das angibt, ob das Gelände auf der Karte flach (deaktiviert) sein soll. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, ob die ursprünglichen Farben in der SVG verwendet werden sollen, anstatt den Paletteneintrag für Farben in einem Bild zu suchen. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement-Eigenschaften

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | 1903 | Beschreibung |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Betrag, um den das Hintergrundelement eines Symbols skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| fillColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe, die für das Füllen von Polygonen, den Hintergrund von Punktsymbolen und für die Mitte von geteilten Linien verwendet wird |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| FontWeight                   | String  |      |      |      |      |  ✔   | Die Dichte einer Schriftart in Bezug auf die Helligkeit oder die schwere haftigkeit der Striche. "**Light**", "**Normal**", "**Semibold**" und "**Bold**" können festgelegt werden. |
| iconColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe der Glyphe, die in der Mitte eines Punktsymbols angezeigt wird |
| iconScale                    | Gleitkomma   |      |  ✔   |  ✔   |  ✔   |  ✔   | Betrag, um den die Glyphe eines Symbols skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| labelColor                   | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der Wert, um den die Standardgröße von Beschriftungen skaliert wird. Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Führt das Überschreiben von **StrokeColor** mit dem Alphawert von **FillColor** aus, anstatt eine Mischung der beiden Farben zu verwenden |
| Dezimalstellen                        | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der Wert, um den die gesamte Größe des Punkts skaliert wird. Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| strokeColor                  | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe, die für die Kontur von Polygonen, die Kontur von Punktsymbolen und die Farbe von Linien verwendet werden soll |
| strokeWidthScale             | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der Wert, um den die Strichstärke von Linien skaliert wird Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Diese Eigenschaftengruppe erbt von der [MapElement](#mapelement)-Eigenschaftengruppe.

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | 1903 | Beschreibung |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die sekundäre oder Umrandungslinienfarbe des Rahmens eines gefüllten Polygons |
| borderStrokeColor            | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die primäre Farbe des Rahmens eines gefüllten Polygons |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Der Betrag, um den der Strich des Rahmens skaliert wird. Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle-Eigenschaften

Diese Eigenschaftengruppe erbt von der [MapElement](#mapelement)-Eigenschaftengruppe.

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | 1903 | Beschreibung |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowvisible                | Bool    |      |      |      |      |  ✔   | Das Flag, das angibt, ob der Schatten des Symbols sichtbar sein soll. |
| Shape-Hintergrund             | String  |      |      |      |      |  ✔   | Form, die als Hintergrund des Symbols verwendet werden soll, und ersetzt jede beliebige Form, die dort vorhanden ist. |
| Form Symbol                   | String  |      |      |      |      |  ✔   | Form, die als Vordergrund Symbol des Symbols verwendet werden soll, und ersetzt jede beliebige Form, die dort vorhanden ist. |
| stemAnchorRadiusScale        | Gleitkomma   |      |      |  ✔   |  ✔   |  ✔   | Betrag, um den der Ankerpunkt eines Symbolschafts skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Größe. |
| stemColor                    | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe des Schafts unten an einem Symbol im 3D-Modus |
| stemHeightScale              | Gleitkomma   |      |      |  ✔   |  ✔   |  ✔   | Betrag, um den die Länge eines Symbolschafts skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Länge. |
| stemOutlineColor             | Farbe   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Die Farbe der Kontur um den Schaft unten an einem Symbol im 3D-Modus |
| stemWidthScale               | Gleitkomma   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Betrag, um den die Breite eines Symbolschafts skaliert werden soll.  Verwenden Sie z. B. *1* für die Standardgröße und *2* für die doppelte Länge. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Diese Eigenschaftengruppe erbt von der [MapElement](#mapelement)-Eigenschaftengruppe.

| Eigenschaft                     | Typ    | 1703 | 1709 | 1803 | 1809 | 1903 | Beschreibung |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | Ein Flag, das angibt, dass ein 3D-Modell wie ein Gebäude gerendert werden soll – ohne Tiefenausblendung gegen den Boden |
