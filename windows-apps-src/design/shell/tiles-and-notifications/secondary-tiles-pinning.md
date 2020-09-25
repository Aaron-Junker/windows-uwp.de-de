---
description: Erfahren Sie, wie Sie eine sekundäre Kachel erstellen und Sie Programm gesteuert über eine universelle Windows-Plattform-app (UWP) an das Startmenü anheften.
title: Anheften von sekundären Kacheln an „Start“
label: Pin secondary tiles to Start
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, sekundäre Kacheln, PIN, anheften, Schnellstart, Codebeispiel, Beispiel, "secondarytile"
ms.localizationpriority: medium
ms.openlocfilehash: 0fc83fca642ae75404180edf5fad177b92153e35
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220383"
---
# <a name="pin-secondary-tiles-to-start"></a>Anheften von sekundären Kacheln an „Start“


In diesem Thema wird Schritt für Schritt erläutert, wie Sie eine sekundäre Kachel für Ihre Windows-app erstellen und an das Startmenü anheften.

![Screenshot der sekundären Kacheln](images/secondarytiles.png)

Weitere Informationen zu sekundären Kacheln finden Sie unter [Übersicht über sekundäre Kacheln](secondary-tiles.md).


## <a name="add-namespace"></a>Namespace hinzufügen

Der Windows. UI. Startscreen-Namespace enthält die secondarytile-Klasse.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Initialisieren der sekundären Kachel

Sekundäre Kacheln bestehen aus einigen wichtigen Komponenten...

* **Tileid**: ein eindeutiger Bezeichner, mit dem Sie die Kachel zwischen den anderen sekundären Kacheln identifizieren können.
* **Display Name**: der Name, der auf der Kachel angezeigt werden soll.
* **Argumente**: die Argumente, die an die APP zurückgegeben werden sollen, wenn der Benutzer auf die Kachel klickt.
* **Square150x150Logo**: das erforderliche Logo, das auf der Kachel mit mittlerer Größe angezeigt wird (und die Größe auf Kachel mit geringer Größe geändert wird, wenn kein kleines Logo bereitgestellt wird).

Sie **müssen** initialisierte Werte für alle oben genannten Eigenschaften angeben, andernfalls wird eine Ausnahme ausgelöst.

Es gibt eine Vielzahl von Konstruktoren, die Sie verwenden können. Wenn Sie jedoch den Konstruktor verwenden, der die tileid, Display Name, Arguments, square150x150Logo und DesiredSize annimmt, stellen Sie sicher, dass Sie alle erforderlichen Eigenschaften festlegen.

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>Optional: Hinzufügen von Unterstützung für größere Kachel Größen

Wenn Sie auf der sekundären Kachel umfassende Kachel Benachrichtigungen anzeigen möchten, können Sie es dem Benutzer ermöglichen, seine Kachel so zu ändern, dass Sie breit oder groß ist, damit Sie noch mehr Inhalte sehen können.

Um Breite und große Kachel Größen zu aktivieren, müssen Sie *Wide310x150Logo* und *Square310x310Logo*bereitstellen. Außerdem sollten Sie, wenn möglich, den *Square71x71Logo* für die kleine Kachel Größe angeben (andernfalls wird der erforderliche Square150x150Logo für die kleine Kachel herabgestuft).

Sie können auch eine eindeutige *Square44x44Logo*bereitstellen, die optional in der unteren rechten Ecke angezeigt wird, wenn eine Benachrichtigung vorhanden ist. Wenn Sie keinen angeben, wird stattdessen der *Square44x44Logo* von ihrer primären Kachel verwendet.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Optional: Aktivieren Sie das Anzeigen des anzeigen Amens.

Standardmäßig wird der Anzeige Name nicht angezeigt. Fügen Sie den folgenden Code hinzu, um den anzeigen Amen für Mittel/breit/groß anzuzeigen.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Optional: 3D-sekundäre Kacheln
Sie können Ihre sekundäre Kachel für Windows Mixed Reality verbessern, indem Sie 3D-Assets hinzufügen. Benutzer können 3D-Kacheln direkt in Ihre Windows Mixed Reality-Startseite anstatt im Startmenü platzieren, wenn Sie Ihre APP in einer gemischten Reality-Umgebung verwenden. Sie können z. b. 360 °-photosphären erstellen, die direkt in eine 360 °-Foto-Viewer-App verweisen, oder Benutzern das Platzieren eines 3D-Modells in einem Möbel Katalog gestatten, der eine Detailseite zu den Preis-und Farboptionen für dieses Objekt öffnet, wenn diese ausgewählt ist. Informationen zu den ersten Schritten finden Sie in der [Mixed Reality-Entwicklerdokumentation](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Anheften der sekundären Kachel

Zum Schluss eine Anforderung zum Anheften der Kachel. Beachten Sie, dass dies von einem UI-Thread aufgerufen werden muss. Auf dem Desktop wird ein Dialogfeld angezeigt, in dem der Benutzer gefragt wird, ob er die Kachel anheften möchte.

> [!IMPORTANT]
> Wenn Sie eine Windows-Desktop Anwendung mit der Desktop Bridge sind, müssen Sie zunächst einen zusätzlichen Schritt ausführen, wie unter [Pin aus Desktop Anwendung](secondary-tiles-desktop-pinning.md) beschrieben.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Überprüfen, ob eine sekundäre Kachel vorhanden ist

Wenn Ihr Benutzer eine Seite in Ihrer APP besucht, die bereits an den Start angeheftet wurde, sollten Sie stattdessen die Schaltfläche "lösen" anzeigen.

Wenn Sie auswählen, welche Schaltfläche angezeigt werden soll, müssen Sie daher zunächst überprüfen, ob die sekundäre Kachel aktuell fixiert ist.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Lösen einer sekundären Kachel

Wenn die Kachel gerade angeheftet ist und der Benutzer auf die Schaltfläche "lösen" klickt, sollten Sie die Kachel lösen (Löschen).

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Aktualisieren einer sekundären Kachel

Wenn Sie die Logos, den anzeigen Amen oder etwas anderes auf der sekundären Kachel aktualisieren müssen, können Sie *requestupdateasync*verwenden.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Auflisten aller fixierten sekundären Kacheln

Wenn Sie alle Kacheln ermitteln müssen, die Ihr Benutzer angeheftet hat, anstatt *secondarytile. vorhanden*zu verwenden, können Sie alternativ *secondarytile. findallasync ()* verwenden.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Eine Kachel Benachrichtigung senden

Informationen zum Anzeigen von umfangreichen Inhalten auf der Kachel über Kachel Benachrichtigungen finden Sie unter [Senden einer lokalen Kachel Benachrichtigung](sending-a-local-tile-notification.md).


## <a name="related"></a>Verwandte Themen

* [Übersicht über sekundäre Kacheln](secondary-tiles.md)
* [Leitfaden für sekundäre Kacheln](secondary-tiles-guidance.md)
* [Kachel Ressourcen](../../style/app-icons-and-logos.md)
* [Dokumentation zu Kachel Inhalt](create-adaptive-tiles.md)
* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
