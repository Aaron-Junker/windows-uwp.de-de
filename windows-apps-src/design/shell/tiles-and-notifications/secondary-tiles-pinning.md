---
Description: Erfahren Sie, wie eine sekundäre Kachel an Start anheften, aus der UWP-app.
title: Sekundäre Kacheln an Startmenü anheften
label: Pin secondary tiles to Start
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, sekundäre Kacheln, Anheften, anheften, Schnellstart, Codebeispiel, Beispiel, Sekundärkachel
ms.localizationpriority: medium
ms.openlocfilehash: 4bebee86c824242cf031503617d4a880ebbb74df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653155"
---
# <a name="pin-secondary-tiles-to-start"></a>Sekundäre Kacheln an Startmenü anheften


Dieses Thema führt Sie durch die Schritte zum Erstellen einer sekundären Kachel für Ihre UWP-App und Anheften der Kachel an das Startmenü.

![Screenshot von sekundären Kacheln](images/secondarytiles.png)

Weitere Informationen zum Erstellen und Anheften von Sekundärkacheln finden Sie unter [Übersicht über sekundäre Kacheln](secondary-tiles.md).


## <a name="add-namespace"></a>Hinzufügen von Namespaces

Der Windows.UI. StartScreen-Namespace enthält die SecondaryTile-Klasse.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Initialisieren Sie die sekundäre Kachel

Sekundäre Kacheln bestehen aus einigen wichtigen Komponenten...

* **TileId**: Ein eindeutiger Bezeichner, der Sie die Kachel für die anderen sekundären Kacheln identifizieren kann.
* **"DisplayName"**: Der Name, den Sie auf der Kachel angezeigt werden sollen.
* **Argumente**: Die Argumente sollen übergebenen zurück zu Ihrer app klickt der Benutzer auf die Kachel.
* **Square150x150Logo**: Das erforderliche Logo, das angezeigt wird, auf die mittlere Größe Kachel (und zur Kachel "klein" geändert werden, wenn kein kleines Logo angegeben).

Sie **MÜSSEN** initialisierte Werte für alle oben genannten Eigenschaften anbieten, da andernfalls eine Ausnahme ausgelöst wird.

Es gibt eine Vielzahl von Konstruktoren, die Sie verwenden können, das Verwenden des Konstruktors, der TileId, DisplayName, Argumente, Square150x150Logo und DesiredSize annimmt, stellt allerdings sicher, dass Sie alle erforderlichen Eigenschaften festlegen.

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


## <a name="optional-add-support-for-larger-tile-sizes"></a>Optional: Hinzufügen von Unterstützung für größere-Kachel

Wenn Sie vorhaben, umfassende Kachelbenachrichtigungen auf der sekundären Kachel anzuzeigen, sollten Sie dem Anwender ermöglichen, die Breite oder Größe ihrer Kachel zu ändern, damit noch mehr Inhalte angezeigt werden.

Um breite und große Kachelgrößen zu aktivieren, müssen Sie *Wide310x150Logo* und *Square310x310Logo* angeben. Wenn möglich, sollten Sie ebenfalls *Square71x71Logo* für die kleine Kachelgröße bereitstellen (andernfalls wird die erforderliche Größe Square150x150Logo für die kleine Kachel angewandt).

Sie können ebenfalls ein eindeutiges *Square44x44Logo* bereitstellen, das optional in der unteren rechten Ecke angezeigt wird, wenn eine Benachrichtigung vorhanden ist. Wenn Sie diese nicht angeben, wird stattdessen *Square44x44Logo* aus der primären Kachel verwendet.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Optional: Aktivieren Sie die Anzeige des Anzeigenamens

Der Anzeigename wird standardmäßig nicht angezeigt. Um den Anzeigenamen in mittelgroß, breit, groß anzuzeigen, fügen Sie den folgenden Code hinzu.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Optional: 3D, sekundäre Kacheln
Sie können die sekundäre Kachel für Windows Mixed Reality verbessern, indem Sie 3D-Ressourcen hinzufügen. Benutzer können 3D-Kacheln direkt in ihre Windows Mixed Reality Home-Umgebung anstelle des Startmenüs platzieren, wenn sie Ihre App in einer Mixed Reality-Umgebung verwenden. Sie können z. B. 360° Photospheres erstellen, die direkt in einer 360°-Foto-App verknüpft werden oder Ihren Benutzern ermöglichen, ein 3D-Modell eines Stuhls aus einem Möbelkatalog zu platzieren, das eine Seite zu den Optionen für Preise und Farben für das Objekt öffnet, wenn es ausgewählt wird. Informationen zu ersten Schritten finden Sie in der [Mixed Reality-Entwicklerdokumentation](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Anheften der sekundären Kachel

Fordern Sie schließlich das Anheften der Kachel auf. Beachten Sie, dass dies von einem UI-Thread aufgerufen werden muss. Auf dem Desktop wird ein Dialogfeld angezeigt, das den Benutzer auffordert zu bestätigen, ob er die Kachel anheften möchte.

> [!IMPORTANT]
> Bei einer Windows-Desktopanwendung, die die Desktop-Brücke verwendet, müssen Sie zuerst einen zusätzlichen Schritt ausführen, wie unter [Anheften ab einer Desktopanwendung](secondary-tiles-desktop-pinning.md) beschrieben

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Überprüfen Sie, ob eine sekundäre Kachel vorhanden ist

Wenn Ihre Benutzer eine Seite in Ihrer App besuchen, die sie bereits auf der Startseite angeheftet haben, möchten Sie stattdessen eine Schaltfläche "Entfernen" anzeigen.

Daher sollten Sie bei der Auswahl der Schaltfläche, die angezeigt werden soll zunächst überprüfen, ob derzeit deren sekundäre Kachel angeheftet ist.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Lösen einer sekundären Kachel

Wenn die Kachel zurzeit angeheftet ist und der Benutzer klickt auf die Schaltfläche „Lösen”, sollte die Kachel gelöst (gelöscht) werden.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Aktualisieren einer sekundären Kachel

Wenn Sie die Logos, Anzeigenamen oder anderen Elementen auf der sekundären Kachel aktualisieren müssen, können Sie *RequestUpdateAsync* verwenden.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Auflisten aller angehefteten sekundären Kacheln

Wenn Sie alle Kacheln ermitteln müssen, die der Benutzer angeheftet hat, verwenden Sie anstelle von *SecondaryTile.Exists* *SecondaryTile.FindAllAsync()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Senden einer Kachelbenachrichtigungen

Weitere Informationen zum Anzeigen umfassender Inhalte auf einer Kachel per Kachelbenachrichtigungen finden Sie unter [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md).


## <a name="related"></a>Verwandte Themen

* [Übersicht über die sekundäre Kacheln](secondary-tiles.md)
* [Sekundäre Kacheln Anleitungen](secondary-tiles-guidance.md)
* [Kachel "-Objekte](app-assets.md)
* [Kachel "Content-Dokumentation](create-adaptive-tiles.md)
* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
