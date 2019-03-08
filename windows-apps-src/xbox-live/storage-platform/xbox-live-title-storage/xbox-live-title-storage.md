---
title: Xbox Live-Titel-Speicher
description: Erfahren Sie, wie Xbox Live Titel Speicher zu verwenden, um Informationen zu Spielen eines Titels in der Cloud zu speichern.
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 472a2131c113cdde5d7383e169e4fbe638e4e555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623925"
---
# <a name="xbox-live-title-storage"></a>Xbox Live-Titel-Speicher

Die Xbox Live Title-Storage-Dienst bietet eine Möglichkeit zum Speichern von Informationen zu Spielen eines Titels in der Cloud. Spiele, die auf allen Plattformen ausgeführt wird, können diesen Dienst verwenden.

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Funktionen von Xbox Live Titel storage

Einige der allgemeinen Features von Xbox Live Titel Storage enthalten, aber Sie sind nicht darauf beschränkt:

-   Können für Benutzer, Titel und verschiedenen Plattformen gemeinsam genutzt werden
-   Unterstützt das Binär, JSON und Konfigurationsdateien

Die wichtigsten Funktionen von Xbox Live-Titel-Speicher werden in den folgenden Abschnitten ausführlicher erläutert:

-   [Arten von Speicher](#ID4ETB)
-   [Arten von Daten](#ID4ECF)
-   [Titel-Speicher-URIs](#ID4EBEAC)
-   [Drosselungsgrenzwert](#ID4ETEAC)

<a name="ID4ETB"></a>

Für verwaltete Partner und ID@Xbox Mitglieder:

| Speichertyp       | Kontingent (verwaltete Partners/ID@Xbox) | Kontingent (Xbox Live Creators-Programm) |  Zweck                                                                                                                                                      | Plattformen                                                                                           | Benutzer                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| Vertrauenswürdige Plattform   | 256 MB pro Benutzer | 64 MB pro Benutzer    | Benutzerspezifische Daten, z. B. gespeichert Spiele oder Spielzustands für abspielen/anhalten/fortsetzen. Sicherer, aber mit plattformeinschränkungen. | Jede Plattform lesen kann, aber nur Xbox One, Xbox 360 oder Windows Phone schreiben kann.  | Konfigurierbar in öffentliche oder nur der Besitzer.       |
| Universelle-Plattform | 64 MB pro Benutzer | 64 MB pro Benutzer    | Benutzerspezifische Daten, z. B. gespeichert Spiele oder Spielzustands für abspielen/anhalten/fortsetzen. | Jede Plattform schreiben, aber nur andere Plattformen als Xbox One, Xbox 360 oder Windows Phone lesen können. | Konfigurierbar in öffentliche oder nur der Besitzer.       |
| Global             | 256 MB | 256 MB            | Daten, die jeder, z. B. Dienstplänen, Zuordnungen, Herausforderungen und Art von Ressourcen lesen kann. | Kann über die Xbox-Entwicklerportal oder die Partner Center geschrieben, einer beliebigen Plattform schreibgeschützt.                                | Alle Benutzer dürfen lesen.

### <a name="deprecated-storage-types"></a>Als veraltet markierte Speichertypen

Die folgenden Speichertypen sind veraltet. Sie werden nur für Titel unterstützt, die derzeit diese verwenden. Sie sind nicht verfügbar, für den Titel.

| Speichertyp       | Kontingent  |   Zweck                                                                                                                                                      | Plattformen                                                                                           | Benutzer                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 64 MB pro Benutzer     | Benutzerspezifische Daten, z. B. gespeichert Spiele oder Spielzustands für abspielen/anhalten/fortsetzen. Mehr Sicherheit, keine plattformeinschränkungen, aber mit Daten formatieren Einschränkungen (nur für JSON). | Jede Plattform möglicherweise lesen oder schreiben.                                                                     | Konfigurierbar in öffentliche oder nur der Besitzer.       |
| Gerät             | 64 MB pro Gerät   | Daten, die auf einem Gerät wie z. B. Einstellungen oder geräteeinstellungen spezifisch sind.                                                                                            | Nur Xbox One, Xbox 360 oder Windows Phone schreiben kann. Es darf nur das Gerät, das die Daten geschrieben lesen.  | Alle Benutzer dürfen lesen.                         |
| Sitzungsspeicher    | 256 MB pro Sitzung | Daten für alle Benutzer mit einer bestimmten multiplayer-Spiele-Sitzung verknüpft ist.                                                                                             | Alle Plattformen, die die Sitzung beitreten kann.                                                             | Alle Benutzer in der Sitzung möglicherweise lesen oder schreiben. |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>Arten von Daten

Spiele geben den Typ der Daten in die **{Type}** Parameter einer Get- oder PUT-Methode. Der folgende Abschnitt beschreibt die drei unterstützten Typen:

-   [Binäre Informationen](#ID4ENF)
-   [JSON-Informationen](#ID4EUF)
-   [Informationen zur Konfiguration](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>Binäre Informationen

Bilder, Sounds und benutzerdefinierte Daten verwenden den binary-Typ. Da die Daten über HTTP übertragen werden müssen, müssen Binärdaten in Zeichen codiert werden, die HTTP akzeptiert. Sie können z. B. die Daten in Zeichenfolgen mit hexadezimal konvertieren oder base64-Codierung verwenden. Das Title-Speichersystem verarbeitet die codierten Daten, damit Ihr Spiel auf dasselbe Schema verwenden muss, zum Codieren und Decodieren von Daten beim Lesen und Schreiben in den Speicher der Titel nicht ab.

<a name="ID4EUF"></a>

#### <a name="json-information"></a>JSON-Informationen

Strukturierte Daten können die JSON-Typ. JSON-Objekte können direkt in Sprachen wie JavaScript verwendet werden, die sie unterstützen. Beim Abrufen von Daten aus JSON-Dateien kann das Spiel Bereitstellen einer *wählen* Parameter, um bestimmte Elemente innerhalb der Struktur zurückzugeben. Verwenden Sie z. B. eine JSON-formatierte Datei mit den folgenden Informationen an:

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| Hinweis                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aus Sicherheitsgründen muss das erste Element der JSON-Daten nicht auf ein Array sein. Mit einem Array im Stammverzeichnis übermittelte JSON-Daten werden vom Dienst abgelehnt. |

Spiele können Teile dieser Struktur mit einer Abfrage wie folgt auswählen:

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

Der Antworttext für diese Abfrage ist:

    {
        "name" : "poison"
    }

Das Array kann mit einer Abfrage wie folgt zugegriffen werden:

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

Der Antworttext für diese Abfrage ist:

    {
        "quest" : "queen"
    }

Die folgenden längeneinschränkungen gelten für JSON-Daten:

-   Numerischer Wert, maximale Länge = 32
-   Zeichenfolge, die maximale Länge = 1024
-   Namen der Eigenschaft, die maximale Länge = 64
-   Hierarchie, die maximale Tiefe = 16
-   Array, maximale Größe = 1024
-   Maximale in einem Objekt untergeordnete Eigenschaften = 1024

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>Informationen zur Konfiguration

Die **{Type}** kann **Config** , um anzugeben, dass die Daten einer Konfigurations-Blob. Blobs für die Konfiguration sind Datenstrukturen, die im globalen Title-Speicher gespeichert sind. Das Format des BLOBs ist ein JSON-Objekt ähnelt.

Konfiguration von Blobs können virtuelle Knoten enthalten, die eine Einstellung aus einer Liste von Möglichkeiten zurückgeben. Virtuelle Knoten eignen sich für die Bereitstellung von Einstellungen für bestimmte Situationen, z. B. für einen Titel oder Gebietsschema. Der virtuelle Knoten enthält mehrere mögliche Einstellungen zusammen mit Werten und Bedingungen für die Auswahl aus den Werten. Im folgenden Beispiel die **DefaultCardDesign** Einstellung kann einen der Werte in den virtuellen Knoten aufweisen.

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

Wenn ein Spiel diese Datei liest, wird das System wählt einen der Werte aus der  **\_SourceNodes** Array. In diesem Fall wird das Element ausgewählt, basierend auf der Title-ID des Spiels. Benutzer des Spiels **12456799** finden Sie unter:

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

Die restlichen Benutzer finden Sie unter:

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

Spiele können benutzerdefinierte Selektoren definieren, die mit einem Parameter in der Anforderung übereinstimmen. Zum Beispiel in diesem Config-Blob:

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

Spiele übergeben eine Zeichenfolge der **CustomSelector** Parameter, auf welches Element zurückgegeben. Beispielsweise rufen Sie die zweite Option fordert ein Spiel:

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

Die  **\_BY** Wert gibt an, welche Art von Auswahl möchten und die  **\_Selektor** Wert gibt an, die Daten für die Verwendung in der Auswahl. Die möglichen Werte sind:

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p>Die <strong>_selector</strong> entspricht der ID "Titel" in dem bereitgestellten Anspruch.</p></td>
</tr>
<tr>
<td >locale</td>
<td ><p>Die <strong>_selector</strong> der gebietsschemazeichenfolge aus dem Accept-Language-Header entspricht.</p></td>
</tr>
<tr>
<td >Benutzerdefinierte</td>
<td ><p>Die <strong>_selector</strong> entspricht einer benutzerdefinierte Zeichenfolge übergeben, die der <strong>CustomSelector</strong> Abfrageparameter. Die <strong>CustomSelector</strong> enthält eine oder mehrere Abfragen, die durch Kommas getrennt. Jede Abfrage entspricht dem Namen aus der <strong>BY</strong> -Element und den Wert aus der <strong>_selector</strong> Element.</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>Titel-Speicher-URIs

Titel Storage URIs sind wie folgt formatiert:

    https://titlestorage.xboxlive.com/{path}

Die **{Path}** -Teil des URI ist der Typ der Anforderung und 245 Zeichen lang sein.

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>Drosselungsgrenzwert

Es gibt keine festen Grenzwerte für wie viele Lese- oder Schreibvorgänge, die ein Titel pro Minute vornehmen kann, aber es in der Regel keine vornehmen mehr als eins pro Minute im Durchschnitt in einer einstündigen Sitzung. Ein Titel kann z. B. 60 Lese- oder Schreibvorgänge zu Beginn einer Sitzung jedoch nicht mehr für den Rest der Stunde vornehmen. Titel sollte später für weitere Aufrufe abgesichert werden, Xbox LIVE Services muss die Anforderungen zu drosseln.

Verfügt Ihre Titel spezielle Partitionierung Voraussetzungen, z. B. zusätzliche Lese- oder Schreibvorgänge, wenden Sie sich an Microsoft.

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>Mit dem Titel-Speicher

Um mit Titel Storage beginnen, bestimmen Sie zunächst welche Art von Daten, die Sie speichern möchten. Beispiele hierfür sind gespeicherte Spiele, Spielzustands, tägliche Herausforderungen, Spiel Karten und Art von Ressourcen.

Als Nächstes zu bestimmen, welche Titel und Plattformen für den Datenzugriff müssen werden. Titel-Speicher unterstützt Cloud Datenzugriff über einen einzelnen Titel auf einer einzigen Plattform und über mehrere Titel auf mehreren Plattformen.

Abschließend verwenden Sie die Themen in diesem Abschnitt Konfigurieren Sie Ihren Speicher, Ihre Daten hochladen und Festlegen von Zugriffsberechtigungen, die entsprechend je nach Ihrer Auswahl.

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[Lesen eines Konfigurations-BLOBs im Speicher für Xbox Live-Titel](reading-configuration-blobs.md)  
Wird das Lesen veranschaulicht Konfiguration Blobs von Xbox Live-Titel Speicher.

[Speichern ein binäres Blob im Speicher für Xbox Live-Titel](storing-binary-blobs.md)  
Zeigt an, speichern binäre Blobs im Speicher für Xbox Live Titel.

[Lesen Sie ein binäres Blob im Speicher für Xbox Live-Titel](reading-binary-blobs.md)  
Zeigt an, lesen binäre Blobs aus dem Title-Xbox Live-Speicher.

[Speichern von JSON-Blob im Speicher für Xbox Live-Titel](storing-jsonblobs.md)  
Veranschaulicht das Speichern von JSON-Blobs im Speicher für Xbox Live Titel an.

[Lesen von JSON-Blob im Speicher für Xbox Live-Titel](reading-jsonblobs.md)  
Zeigt beim Lesen von JSON-Blobs von Xbox Live-Titel Speicher.

<a name="ID4E4FAC"></a>
