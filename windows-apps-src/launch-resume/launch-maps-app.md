---
title: Starten der Windows-Karten-App
description: Erfahren Sie, wie Sie die Windows Maps-APP mithilfe der URI-Schemas bingmaps, MS-Drive-to, MS-Walk-to und MS-Settings von ihrer app starten.
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e020972a8dff0b0721fd2c5726999a7896d359c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167904"
---
# <a name="launch-the-windows-maps-app"></a>Starten der Windows-Karten-App




Erfahren Sie, wie Sie die Windows-Karten-App aus Ihrer App starten können. In diesem Thema werden die **bingmaps:**-, **ms-drive-to:**-, **ms-walk-to:**- und **ms-settings:**-Uniform Resource Identifier (URI)-Schemas erläutert. Verwenden Sie diese URI-Schemas zum Starten der Windows-Karten-App für bestimmte Karten, Wegbeschreibungen und Suchergebnisse oder um über die Einstellungs-App Offlinekarten für Windows-Karten herunterzuladen.

**Tipp** Um mehr über das Starten der Windows-Karten-App aus Ihrer App zu erfahren, laden Sie das [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) aus dem Repository [Beispiele für universelle Windows-Plattform](https://github.com/Microsoft/Windows-universal-samples) auf GitHub herunter.

## <a name="introducing-uris"></a>Einführung in URIs

Mit URI-Schemas können Sie Apps öffnen, indem Sie auf Hyperlinks klicken (oder programmgesteuert in Ihrer App). Ebenso wie Sie eine neue E-Mail mithilfe von **mailto:** starten oder einen Webbrowser mithilfe von **http:** verwenden können, haben Sie auch die Möglichkeit, die Windows-Karten-App mithilfe von **bingmaps:**, **ms-drive-to:** und **ms-walk-to:** zu öffnen.

-   Der **bingmaps:**-URI stellt Karten für Standorte, Suchergebnisse, Wegbeschreibungen und Datenverkehr bereit.
-   Der **ms-drive-to:**-URI stellt detaillierte Fahrtwegbeschreibungen von Ihrem aktuellen Standort bereit.
-   Der **ms-walk-to:**-URI stellt detaillierte Fußwegbeschreibungen von Ihrem aktuellen Standort bereit.

Der folgende URI startet beispielsweise die Windows-Karten-App und zeigt eine über New York City zentrierte Karte an.

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![Eine über New York City zentrierte Karte.](images/mapnyc.png)

Beschreibung des URI-Schemas:

**bingmaps:?query**

In diesem URI-Schema steht *query* für eine Reihe von Parametername-/-wert-Paaren:

**&Param1 = Value1&Param2 = Value2...**

Eine vollständige Liste der verfügbaren Parameter finden Sie unter dem [bingmaps:](#bingmaps-param-reference)-, [ms-drive-to:](#ms-drive-to-param-reference)- und [ms-walk-to:](#ms-walk-to-param-reference)-Parameterverweis. Weiter unten in diesem Thema finden Sie dazu Beispiele.

## <a name="launch-a-uri-from-your-app"></a>Starten eines URIs aus Ihrer App


Um die Windows-Karten-App aus Ihrer App zu starten, rufen Sie die [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)-Methode mit einem **bingmaps:**-, **ms-drive-to:**- oder **ms-walk-to:**-URI auf. Im folgenden Beispiel wird derselbe URI wie im vorherigen Beispiel gestartet. Weitere Informationen zum Starten von Apps mithilfe des URIs finden Sie unter [Starten der Standard-App für einen URI](launch-default-app.md).

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

In diesem Beispiel wird die [**LauncherOptions**](/uwp/api/Windows.System.LauncherOptions)-Klasse verwendet, um sicherzustellen, dass die Windows-Karten-App gestartet wird.

## <a name="display-known-locations"></a>Anzeigen bekannter Positionen

Es gibt zahlreiche Optionen, um zu steuern, welcher Teil der Karte angezeigt werden soll. Sie können den *CP* -Parameter (Center Point) mit den Parametern " *Rad* " (RADIUS) oder " *LVL* " (Zoomstufe) verwenden, um einen Speicherort anzuzeigen und zu wählen, wie nahe er vergrößert werden soll. Wenn Sie den *CP* -Parameter verwenden, können Sie auch ein *HDG* (Überschrift) und eine *Pit* (Tonhöhe) angeben, um zu steuern, welche Richtung gesucht werden soll. Eine andere Methode besteht darin, den Parameter " *BB* " (Begrenzungs Bereich) zu verwenden, um die maximalen Süd-, Ost-, Nord-und West Koordinaten des Bereichs anzugeben, den Sie anzeigen möchten.

Um den Ansichtstyp zu steuern, verwenden Sie die Parameter *sty* (Style) und *SS* (Streetside). Mit dem *sty* -Parameter können Sie zwischen den Ansichten Straße und Luftbild wechseln. Der *ss*-Parameter fügt die Karte in einer Streetside-Ansicht ein. Weitere Informationen zu diesen und anderen Parametern finden Sie unter [bingmaps: Parameterverweis](#bingmaps-param-reference).


| Beispiel-URI                                                                 | Ergebnisse                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | Öffnet die Karten-App.                                                                                                                                                                            |
| bingmaps:?cp=40.726966~-74.006076                                          | Zeigt eine über New York City zentrierte Karte an.                                                                                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&lvl=10                                   | Zeigt eine über New York City zentrierte Karte mit Zoomfaktor 10 an.                                                                                                                            |
| bingmaps:? BB = 39.719 \_ -74.52 ~ 41.71 \_ -73,5                                   | Zeigt eine Karte von New York City an, bei der es sich um den im **BB** -Argument angegebenen Bereich handelt.                                                                                                           |
| bingmaps:? BB = 39.719 \_ -74.52 ~ 41.71 \_ -73,5&CP = 47 ~-122                        | Zeigt eine Karte von New York City an, die dem im Begrenzungsrahmenargument angegebenen Bereich entspricht. Der Mittelpunkt für Seattle, der im **CP** -Argument angegeben ist, wird ignoriert, da *BB* angegeben wird. |
| bingmaps:? Collection = Point. 36.116584 \_ -115,176753 \_ Caesars% 20palace&LVL = 16 | Zeigt den Punkt „Caesar’s Palace“ (in Las Vegas) auf einer Karte mit Zoomfaktor 16 an.                                                                                                 |
| bingmaps:? Collection = Point. 40.726966 \_ -74,006076 \_ some %255 bebusiness        | Zeigt eine Karte mit einem Punkt mit dem Namen some \_ Business an (in Las Vegas).                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&trfc=1&sty=a                             | Zeigt eine Karte von New York City mit Datenverkehr und dem Luftbild an.                                                                                                                          |
| bingmaps:?cp=47.6204~-122.3491&sty=3d                                      | Zeigt eine 3D-Ansicht der Space Needle an.                                                                                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&sty=3d&rad=200&pit=75&hdg=165               | Zeigt eine 3D-Ansicht der Leerraum Nadel mit einem Radius von 200M, einer Höhe von 75 Grad und einer Überschrift von 165 Grad an.                                                                             |
| bingmaps:?cp=47.6204~-122.3491&ss=1                                        | Zeigt eine Streetside-Ansicht der Space Needle an.                                                                                                                                                |


## <a name="display-search-results"></a>Anzeigen von Suchergebnissen

Wenn Sie mithilfe des *q* -Parameters nach Stellen suchen, empfiehlt es sich, die Begriffe so spezifisch wie möglich zu gestalten und einen Such Speicherort mit den Parametern *CP*, *BB*oder *Where* anzugeben. Wenn Sie keinen Such Speicherort angeben und der aktuelle Speicherort des Benutzers nicht verfügbar ist, gibt die Suche möglicherweise keine aussagekräftigen Ergebnisse zurück. Die Suchergebnisse werden in der am besten geeigneten Kartenansicht angezeigt. Weitere Informationen zu diesen und anderen Parametern finden Sie unter [bingmaps: Parameterverweis](#bingmaps-param-reference).


| Beispiel-URI                                                    | Ergebnisse                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:? q = 1600% 20pennsylvania% 20ave,% 20washington,% 20dc     | Zeigt eine Karte an und sucht die Adresse des Weißen Hauses in Washington, D.C. |
| bingmaps:?q=coffee&where=Seattle                              | Sucht nach „Kaffee“ in Seattle.                                                    |
| bingmaps:? CP = 40.726966 ~-74,006076&WHERE = New% 20York            | Sucht nach New York in der Nähe des angegebenen Mittelpunkts.                             |
| bingmaps:? BB = 39.719 \_ -74.52 ~ 41.71 \_ -73,5&q = Pizza              | Sucht im angegebenen umgebenden Feld nach Pizza (in New York City).      |

 
## <a name="display-multiple-points"></a>Anzeigen mehrerer Punkte


Verwenden Sie den *collection*-Parameter zum Anzeigen eines benutzerdefinierten Satzes von Punkten auf der Karte. Wenn mehr als ein Punkt vorhanden ist, wird eine Punktliste angezeigt. Eine Sammlung kann bis zu 25 Punkte umfassen, die in der angegebenen Reihenfolge aufgeführt sind. Die Sammlung hat Vorrang vor den Such- und Routenanforderungen. Weitere Informationen zu diesen und anderen Parametern finden Sie unter [bingmaps: Parameterverweis](#bingmaps-param-reference).

| Beispiel-URI | Ergebnisse                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps:? Collection = Point. 36.116584 \_ -115,176753 \_ Caesars% 20palace                                                                                                | Sucht nach „Caesar’s Palace“ in Las Vegas und zeigt die Ergebnisse in der am besten geeigneten Kartenansicht auf einer Karte an.                         |
| bingmaps:? Collection = Point. 36.116584 \_ -115,176753 \_ Caesars% 20palace&LVL = 16                                                                                         | Zeigt die Ortsmarke „Caesars Palace“ in Las Vegas mit dem Zoomfaktor 16 an.                                               |
| bingmaps:? Collection = Point. 36.116584 \_ -115,176753 \_ Caesars% 20palace ~ Point. 36.113126 \_ -115,175188 \_ The% 20bellagio&LVL = 16&CP = 36.114902 ~-115,176669                   | Zeit die beiden Ortsmarken „Caesars Palace“ und „The Bellagio“ in Las Vegas mit dem Zoomfaktor 16 an.              |
| bingmaps:? Collection = Point. 40.726966 \_ -74,006076 \_ Fake %255 f Business %255 f with %255 funderscore                                                                        | Zeigt New York mit einem PushPin namens Fake \_ Business \_ mit unter \_ Strich an.                                                  |
| bingmaps:? Collection = Name. Hotel% 20list ~ Point. 36.116584 \_ -115,176753 \_ Caesars% 20palace ~ Point. 36.113126 \_ -115,175188 \_ The% 20bellagio&LVL = 16&CP = 36.114902 ~-115,176669 | Zeigt die Liste „Hotel List“ und die beiden Ortsmarken „Caesars Palace“ und „The Bellagio“ in Las Vegas mit dem Zoomfaktor 16 an. |

 

## <a name="display-directions-and-traffic"></a>Anzeigen von Wegbeschreibungen und Verkehr


Mithilfe des *RTP* -Parameters können Sie Umleitungen zwischen zwei Punkten anzeigen. Diese Punkte können entweder Adressen oder breiten-und Längenkoordinaten sein. Verwenden Sie den *trfc*-Parameter zum Anzeigen von Verkehrsinformationen. Verwenden Sie den *mode*-Parameter, um die Art der Anweisungen anzugeben (fahren, laufen oder öffentliche Verkehrsmittel). Wenn *Mode* nicht angegeben wird, werden die Anweisungen im bevorzugten Transportmodus des Benutzers bereitgestellt. Weitere Informationen zu diesen und anderen Parametern finden Sie unter [bingmaps: Parameterverweis](#bingmaps-param-reference).

![Beispiel für eine Wegbeschreibung](images/windowsmapgcdirections.png)

| Beispiel-URI                                                                                                              | Ergebnisse                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:? RTP = POS. 44.9160 \_ -110.4158 ~ POS. 45.0475 \_ -109,4187                                                             | Zeigt eine Karte mit Punkt-zu-Punkt-Wegbeschreibungen an. Da *mode* nicht angegeben ist, werden Wegbeschreibungen mithilfe der Einstellung für die Transportmethode des Benutzers bereitgestellt. |
| bingmaps:?cp=43.0332~-87.9167&trfc=1                                                                                    | Zeigt eine über Milwaukee, Wisconsin, USA zentrierte Karte mit Verkehr an.                                                                                                        |
| bingmaps:? RTP = AdR. One Microsoft Way, Redmond, WA 98052 ~ POS. 39.0731 \_ -108,7238                                           | Zeigt eine Karte mit Wegbeschreibungen von der angegebenen Adresse zur angegebenen Position an.                                                                            |
| bingmaps:? RTP = AdR. 1% 20microsoft% 20Way,% 20redmond,% 20wa, %2098052 ~ POS. 36.1223 \_ -111,9495, \_ Gesamt% 20canyon% 20nord% 20rim | Zeigt eine Wegbeschreibung von 1 Microsoft Way, Redmond, Washington, 98052, USA zum nördlichen Rand des Grand Canyon an.                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | Zeigt eine Karte mit Anfahrtsbeschreibungen von der angegebenen Position zum angegebenen Orientierungspunkt an.                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=d                      | Zeigt die Anfahrtsbeschreibung von Mountain View, Kalifornien, USA nach San Francisco International Airport, Kalifornien, USA an.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=w                      | Zeigt die Fußwegbeschreibungen von Mountain View, Kalifornien, USA nach San Francisco International Airport, Kalifornien, USA an.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=t                      | Zeigt Wegbeschreibungen für öffentliche Verkehrsmittel von Mountain View, Kalifornien, USA nach San Francisco International Airport, Kalifornien, USA an.                                                                  |

## <a name="display-turn-by-turn-directions"></a>Anzeigen detaillierter Wegbeschreibungen


Mit den **ms-drive-to:**- und **ms-walk-to:**-URI-Schemas können Sie direkt eine detaillierte Ansicht einer Route starten. Diese URI-Schemas können nur Wegbeschreibungen von der aktuellen Position des Benutzers bereitstellen. Wenn Sie Wegbeschreibungen zwischen Punkten bereitstellen müssen, die nicht die aktuelle Position des Benutzers enthalten, verwenden Sie das **bingmaps:**-URI-Schema wie im vorherigen Abschnitt beschrieben. Weitere Informationen zu diesen URI-Schemas finden Sie unter dem [ms-drive-to:](#ms-drive-to-param-reference)- und [ms-walk-to:](#ms-walk-to-param-reference)-Parameterverweis.

> **Wichtig**  Wenn die URI-Schemas **ms-drive-to:** oder **ms-walk-to:** gestartet werden, überprüft die Karten-App, ob für das Gerät bereits ein GPS-Positionsfixpunkt bestimmt wurde. Ist dies der Fall, geht die Karten-App zu detaillierten Wegbeschreibungen über. Falls nicht, zeigt die App die Routenübersicht an, wie unter [Anzeigen von Wegbeschreibungen und Verkehr](#display-directions-and-traffic) beschrieben.

![Beispiel für eine detaillierte Wegbeschreibung](images/windowsmapsappdirections.png)

| Beispiel-URI                                                                                                | Ergebnisse                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&destination.longitude=-122.328262&destination.name=Green Lake | Zeigt eine Karte mit detaillierten Anfahrtsbeschreibungen von Ihrem aktuellen Standort zu Green Lake. |
| ms-walk-to:?destination.latitude=47.680504&destination.longitude=-122.328262&destination.name=Green Lake  | Zeigt eine Karte mit detaillierten Fußwegbeschreibungen von Ihrem aktuellen Standort zu Green Lake. |


## <a name="download-offline-maps"></a>Herunterladen von Offlinekarten

Mit dem **ms-settings:**-URI-Schema können Sie direkt eine bestimmte Seite in der Einstellungs-App öffnen. Die Karten-App wird zwar nicht durch das **ms-settings:**-URI-Schema gestartet, Sie haben jedoch die Möglichkeit, die Seite für die Offlinekarten direkt in der Einstellungs-App zu starten. Außerdem zeigt das URI-Schema ein Bestätigungsdialogfeld zum Herunterladen der von der Karten-App verwendeten Offlinekarten an. Das URI-Schema akzeptiert einen Punkt, der durch einen Breiten- und Längengrad angegeben wird, und bestimmt automatisch, ob für eine Region mit diesem Punkt Offlinekarten verfügbar sind.  Wenn der übergebene Breiten- und Längengrad in mehrere Download-Regionen fällt, kann der Benutzer im Bestätigungsdialogfeld auswählen, welche dieser Regionen heruntergeladen werden soll. Wenn Offlinekarten für eine Region mit diesem Punkt nicht verfügbar sind, wird die Seite mit den Offlinekarten in der Einstellungs-App mit einem Fehlerdialogfeld angezeigt.

| Beispiel-URI  | Ergebnisse |
|-------------|---------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | Öffnet die Einstellungs-App auf der Offlinekarten-Seite mit einem Bestätigungsdialogfeld, das angezeigt wird, um Karten für die Region herunterzuladen, die den Punkt mit dem angegebenen Breiten- und Längengrad enthält. |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>bingmaps: Parameterverweis

Die Syntax für jeden Parameter in dieser Tabelle wird mithilfe von ABNF (Augmented Backus-Naur Form) angezeigt.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Definition</th>
<th align="left">ABNF-Definition und Beispiel</th>
<th align="left">Details</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><b>erfolgen</b></p></td>
<td align="left"><p>Mittelpunkt</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3digit ["." 1*7digit]</p>
<p>degreeslon = ["-"] 1*2 Ziffern ["." 1*7digit]</p>
<p>Beispiel:</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>Beide Werte müssen in Dezimalgraden ausgedrückt und durch eine Tilde () getrennt werden <b>~</b> .</p>
<p>Gültige Längengradwerte liegen zwischen -180 und einschließlich +180.</p>
<p>Gültige Breitengradwerte liegen zwischen -90 und einschließlich +90.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>BBS</b></p></td>
<td align="left"><p>Begrenzungsrahmen</p></td>
<td align="left"><p>bb = "bb=" southlatitude "_" westlongitude "~" northlatitude "_" eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>Beispiel:</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>Ein rechteckiger Bereich, der das umgebende Feld mithilfe einer Tilde ( <b>~</b> ) zum Trennen der unteren linken Ecke von der oberen rechten Ecke angibt. Breiten- und Längengrad sind jeweils durch einen Unterstrich (<b>_</b>) getrennt.</p>
<p>Gültige Längengradwerte liegen zwischen -180 und einschließlich +180.</p>
<p>Gültige Breitengradwerte liegen zwischen -90 und einschließlich +90.</p><p>Die Parameter „cp“ und „lvl“ werden ignoriert, wenn ein Begrenzungsrahmen bereitgestellt wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>where</b></p></td>
<td align="left"><p>Ort</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1 *(Alpha/Digit/"-"/"."/"_"/PCT-codiert/"!"/"$"/"" "/" ("/") "/"*"/" + "/", "/"; "/": "/" @" / " /"/"? ")</p>
<p>Beispiel:</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>Ein Suchbegriff für eine bestimmte Position, Sehenswürdigkeit oder einen bestimmten Ort.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>Q1</b></p></td>
<td align="left"><p>Abfrageausdruck</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>Beispiel:</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>Suchbegriff für ein lokales Unternehmen oder eine Unternehmenskategorie.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>LVL</b></p></td>
<td align="left"><p>Zoomfaktor</p></td>
<td align="left"><p>lvl = "lvl=" 1<i>2DIGIT ["." 1</i>2DIGIT]</p>
<p>Beispiel:</p>
<p>lvl=10.50</p></td>
<td align="left"><p>Definiert den Zoomfaktor der Kartenansicht. Gültige Werte sind 1 bis 20, wobei 1 durchgehend gezoomt ist.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>Plastik</b></p></td>
<td align="left"><p>Style</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>Beispiel:</p>
<p>sty=a</p></td>
<td align="left"><p>Definiert den Kartenstil. Gültige Werte für diesen Parameter umfassen:</p>
<ul>
<li><b>a</b>: Zeigt eine Luftansicht der Karte an.</li>
<li><b>r</b>: Zeigt eine Straßenansicht der Karte an.</li>
<li><b>3d</b>: Zeigt eine 3D-Ansicht der Karte an. Verwenden Sie den Wert in Verbindung mit dem <b>cp</b>-Parameter und optional mit dem <b>rad</b>-Parameter.</li>
</ul>
<p>Unter Windows 10 sind die Stile der Luftansicht und der 3D-Ansicht identisch.</p>
<div class="alert">
<b>Hinweis</b>    Wenn Sie den <b>sty</b> -Parameter weglassen, werden dieselben Ergebnisse erzielt wie "sty = r".
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>rad</b></p></td>
<td align="left"><p>Radius</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>Beispiel:</p>
<p>rad=1000</p></td>
<td align="left"><p>Eine kreisförmige Fläche, die die gewünschte Kartenansicht festlegt. Der Radiuswert wird in Metern gemessen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>pit</b></p></td>
<td align="left"><p>Neigung</p></td>
<td align="left"><p>pit = "pit=" Nickwinkel</p>
<p>Beispiel:</p>
<p>pit=60</p></td>
<td align="left"><p>Gibt den Winkel der Kartenansicht an, wobei der Wert 90 der Ansicht des Horizonts (Maximum) und der Wert 0 der Draufsicht (Minimum) entspricht.</p><p>Gültige Nickwerte liegen zwischen 0 und einschließlich 90.</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>Überschrift</p></td>
<td align="left"><p>hdg = "hdg=" Richtung</p>
<p>Beispiel:</p>
<p>hdg=180</p></td>
<td align="left"><p>Gibt die Richtung der Karte in Grad an, dabei gilt: 0 oder 360 = Norden, 90 = Osten, 180 = Süden und 270 = Westen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>ss</b></p></td>
<td align="left"><p>Streetside</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>Beispiel:</p>
<p>ss=1</p></td>
<td align="left"><p>Gibt an, dass bei <code>ss=1</code> Straßenbilder angezeigt werden. Wenn der <b>ss</b>-Parameter ausgelassen wird, wird dasselbe Ergebnis wie bei <code>ss=0</code> zurückgegeben. Verwenden Sie den Wert in Verbindung mit dem <b>cp</b>-Parameter, um die Position der Straßenansicht festzulegen.</p>
<div class="alert">
<b>Hinweis</b>    Bilder auf der Straßenebene sind nicht in allen Regionen verfügbar.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>trfc</b></p></td>
<td align="left"><p>Verkehr</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>Beispiel:</p>
<p>trfc=1</p></td>
<td align="left"><p>Gibt an, ob die Karte Verkehrsinformationen enthält. Wenn der trfc-Parameter ausgelassen wird, werden dieselben Ergebnisse wie bei <code>trfc=0</code> zurückgegeben.</p>
<div class="alert">
<b>Hinweis</b>    Datenverkehrs Daten sind nicht in allen Regionen verfügbar.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>rtp</b></p></td>
<td align="left"><p>Route</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1 (Alpha/Digit/"-"/"."/"_"/PCT-codiert/"!"/"$"/"" "/" ("/") "/" "/" + "/", "/"; "/": "/" @" / " /"/"? ")</p>


<p>Beispiele:</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>RTP = AdR. Eine% 20microsoft% 20Way,% 20redmond,% 20wa ~ POS. 45.23423 _-122.1232_My% 20picnic% 20spot</p></td>
<td align="left"><p>Definiert den Anfang und das Ende einer Route, die auf der Karte gezeichnet werden soll, getrennt durch eine Tilde ( <b>~</b> ). Jeder Wegpunkt wird entweder durch eine Position mittels Längengrad, Breitengrad und optionalem Titel oder durch einen Adressbezeichner definiert.</p>
<p>Eine vollständige Route enthält genau zwei Wegpunkte. Beispielsweise wird eine Route mit zwei Wegpunkten von <code>rtp="A"~"B"</code> definiert.</p>
<p>Außerdem ist es zulässig, eine unvollständige Route anzugeben. Beispielsweise können Sie mit <code>rtp="A"~</code> nur den Startpunkt einer Route definieren. In diesem Fall wird die Wegbeschreibungseingabe so angezeigt, dass das Feld <b>Von:</b> den angegebenen Wegpunkt enthält und das Feld <b>Nach:</b> den Fokus hat.</p>
<p>Wenn nur der Endpunkt der Route angegeben wird, wie bei <code>rtp=~"B"</code>, wird der Bereich für die Wegbeschreibung mit dem angegebenen Wegpunkt im Feld <b>Nach:</b> angezeigt. Wenn ein genauer aktueller Standort verfügbar ist, wird die aktuelle Position automatisch im Feld <b>Von</b> ausgefüllt und hat den Fokus.</p>
<p>Bei einer unvollständigen Route wird keine Routenlinie gezeichnet.</p>
<p>Geben Sie dies zusammen mit dem <b>mode</b>-Parameter an, um die Transportmethode (fahren, laufen oder öffentliche Verkehrsmittel) anzugeben. Wenn <b>mode</b> nicht angegeben ist, werden Wegbeschreibungen mithilfe der Einstellung für die Transportmethode des Benutzers bereitgestellt.</p>
<div class="alert">
<b>Hinweis</b>    Ein Titel kann für einen Speicherort verwendet werden, wenn der Speicherort durch den <b>POS</b> -Parameterwert angegeben wird. Anstatt der Breiten- und Längengrade wird der Titel angezeigt.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mode</b></p></td>
<td align="left"><p>Transportmethode</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>Beispiel:</p>
<p>mode=d</p></td>
<td align="left"><p>Legt die Transportmethode fest. Gültige Werte für diesen Parameter umfassen:</p>
<ul>
<li><b>d</b>: Zeigt eine Routenübersicht für Wegbeschreibungen an.</li>
<li><b>t</b>: Zeigt eine Routenübersicht für Wegbeschreibungen für öffentliche Verkehrsmittel an.</li>
<li><b>w</b>: Zeigt eine Routenübersicht für Fußwegbeschreibungen an.</li>
</ul>
<p>Verwenden Sie den Wert in Verbindung mit dem <b>rtp</b>-Parameter für die Routenbeschreibungen. Wenn <b>mode</b> nicht angegeben ist, werden Wegbeschreibungen mithilfe der Einstellung für die Transportmethode des Benutzers bereitgestellt. Ein <b>mode</b>-Parameter kann ohne Routenparameter bereitgestellt werden, um die Wegbeschreibungseingabe für diese Methode vom aktuellen Standort einzugeben.</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>Ausstellung</b></p></td>
<td align="left"><p>Collection</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1 (Alpha/Digit/"-"/"."/"_"/PCT-codiert/"!"/"$"/"" "/" ("/") "/" "/" + "/", "/"; "/": "/" @" / " /"/"? ") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>Beispiel:</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>Eine Sammlung von Punkten, die der Karte und Liste hinzugefügt werden sollen. Die Sammlung von Punkten kann mithilfe des Namensparameters benannt werden. Ein Punkt wird mithilfe eines Breitengrads, Längengrads und eines optionalen Titels angegeben.</p>
<p>Separater Name und mehrere Punkte mit Tilden ( <b>~</b> ).</p>
<p>Wenn das angegebene Element eine Tilde enthält, muss die Tilde als <code>%7E</code> codiert sein. Wenn es nicht zusammen mit den Parametern für den Mittelpunkt und Zoomfaktor angegeben wird, liefert die Sammlung die beste Kartenansicht.</p>

<p><b>Wichtig</b> Wenn das angegebene Element einen Unterstrich enthält, muss der Unterstrich als „%255F“ doppelt verschlüsselt werden.</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>ms-drive-to: Parameterverweis


Der URI zum Starten einer Anforderung für detaillierte Wegbeschreibungen muss nicht codiert werden und hat das folgende Format.

> **Hinweis**  In diesem URI-Schema wird der Startpunkt nicht angegeben. Beim Startpunkt wird immer davon ausgegangen, dass es sich um die aktuelle Position handelt. Wenn Sie einen anderen Startpunkt als die aktuelle Position angeben müssen, finden Sie weitere Informationen unter [Anzeigen von Wegbeschreibungen und Verkehr](#display-directions-and-traffic).

 

| Parameter | Definition | Beispiel | Details |
|------------|-----------|---------|---------|
| **destination.latitude** | Breitengrad des Ziels | Beispiel: destination.latitude=47.6451413797194 | Der Breitengrad des Ziels. Gültige Breitengradwerte liegen zwischen -90 und einschließlich +90. |
| **destination.longitude** | Längengrad des Ziels | Beispiel: destination.longitude=-122.141964733601 | Der Längengrad des Ziels. Gültige Längengradwerte liegen zwischen -180 und einschließlich +180. |
| **destination.name** | Name des Ziels | Beispiel: destination.name=Redmond, WA | Der Name des Ziels. Eine Codierung des URI oder des **destination.name**-Werts ist nicht erforderlich. |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>ms-walk-to: Parameterverweis


Der URI zum Starten einer Anforderung für detaillierte Fußwegbeschreibungen muss nicht codiert werden und hat das folgende Format.

> **Hinweis**  In diesem URI-Schema wird der Startpunkt nicht angegeben. Beim Startpunkt wird immer davon ausgegangen, dass es sich um die aktuelle Position handelt. Wenn Sie einen anderen Startpunkt als die aktuelle Position angeben müssen, finden Sie weitere Informationen unter [Anzeigen von Wegbeschreibungen und Verkehr](#display-directions-and-traffic).
 

| Parameter | Definition | Beispiel | Details |
|-----------|------------|---------|----------|
| **destination.latitude** | Breitengrad des Ziels | Beispiel: destination.latitude=47.6451413797194 | Der Breitengrad des Ziels. Gültige Breitengradwerte liegen zwischen -90 und einschließlich +90. |
| **destination.longitude** | Längengrad des Ziels | Beispiel: destination.longitude=-122.141964733601 | Der Längengrad des Ziels. Gültige Längengradwerte liegen zwischen -180 und einschließlich +180. |
| **destination.name** | Name des Ziels | Beispiel: destination.name=Redmond, WA | Der Name des Ziels. Eine Codierung des URI oder des **destination.name**-Werts ist nicht erforderlich. |

## <a name="ms-settings-parameter-reference"></a>ms-settings: Parameterverweis

Die Syntax für Karten-App-spezifische Parameter für das **ms-settings:**-URI-Schema wird unten definiert. **maps-downloadmaps** wird zusammen mit dem **ms-settings:**-URI in Form von **ms-settings:maps-downloadmaps?** angegeben, um die Offlineseite für Karteneinstellungen anzugeben. 

| Parameter | Definition | Beispiel | Details |
|-----------|------------|---------|----------|
| **latlong** | Punkt, der die Region für die Offlinekarte definiert. | Beispiel: latlong=47.6,-122.3 | Der GeoPoint wird durch einen durch ein Komma getrennten Breiten- und Längengrad angegeben. Gültige Breitengradwerte liegen zwischen -90 und einschließlich +90. Gültige Längengradwerte liegen zwischen -180 und einschließlich +180. |