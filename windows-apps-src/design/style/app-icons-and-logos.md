---
Description: 'Vorgehensweise: Erstellen von app-Symbole/Logos, die Ihre app in das Startmenü, app-Kacheln, die Taskleiste, den Microsoft Store und mehr darstellen.'
title: App-Symbole und Logos
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622455"
---
# <a name="app-icons-and-logos"></a>App-Symbole und Logos 

Jede app hat ein Symbol oder-Logo, die es darstellt, und dieses Symbol angezeigt wird, an mehreren Standorten in der Windows-Shell: 

:::row:::
    :::column:::
        * Die Titelleiste des app-Fensters
        * Der app-Liste im Startmenü
        * Der Taskleiste und Task-manager
        * Ihrer app-Kacheln
        * Begrüßungsbildschirm der app
        * In den Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Dieser Artikel behandelt die Grundlagen der Erstellung von app-Symbole, wie Sie mit Visual Studio, zu verwalten und wie sie manuell verwalten müssen Sie.
 
(In diesem Artikel wird speziell für die Symbole, die die app selbst darstellen; das Symbol für allgemeine Anleitungen finden Sie die [Symbole](icons.md) Artikel.)

## <a name="icon-types-locations-and-scale-factors"></a>Für Symboltypen, Speicherorten und Skalierungsfaktoren

Standardmäßig speichert Visual Studio Medienobjekte Symbol in ein Unterverzeichnis des Assets. Hier ist eine Liste der verschiedenen Typen von Symbolen, wo diese angezeigt werden, und was sie aufgerufen wurden. 

| Symbolname | Angezeigt in | Medienobjekt-Dateiname |
| ---      | ---        | --- |
| Kleine Kachel | Startmenü |  SmallTile.png  |
| Mittelgroße Kachel |Menü "," Start "Microsoft Store-Liste\*  |  Square150x150Logo.png |
| Breite Kachel  | Startmenü   | Wide310x150Logo.png |
| Große Kachel   | Menü "," Start "Microsoft Store-Liste\* |  LargeTile.png  |
| App-Symbol | App-Liste im Menü "Start", Taskleiste, Task-manager | Square44x44Logo.png |
| Begrüßungsbildschirm | Begrüßungsbildschirm der app | SplashScreen.png  |
| Badgelogo | Ihrer app-Kacheln | BadgeLogo.png  |
| Paket-Logo/Store-logo | App-Installer, Partner Center die Option "Eine app Report" in den Store, die Option "Rezension schreiben", in den Store | StoreLogo.png  |

\* Verwendet, wenn Sie die Option [Anzeige von nur hochgeladenen Bildern in der Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Um sicherzustellen, dass diese Symbole in allen Bildschirmen scharf aussehen, können Sie mehrere Versionen des gleichen Symbols für andere Skalierungsfaktoren erstellen. 

Der Skalierungsfaktor bestimmt die Größe der Elemente der Benutzeroberfläche, z. B. Text. Skalieren Sie Faktoren-Bereich von 100 % bis 400 %. Größere Werte erstellen, größere Elemente der Benutzeroberfläche erleichtert das Hochauflösende anzeigen angezeigt wird. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Da app-Symbolressourcen Bitmaps sind und Bitmaps nicht gut skalierbar, empfehlen wir einer Version jedes Symbol-Objekt für jede Skalierungsfaktor bereitstellen: 100 %, 125 %, 150 %, 200 % und 400 %. Das sind viele der Symbole. Glücklicherweise stellt Visual Studio ein Tool, das Generieren und aktualisieren diese Symbole erleichtert. 

## <a name="microsoft-store-listing-image"></a>Bild von Microsoft Store-Liste

"Wie gebe ich Bilder für meine app-Liste in der Microsoft Store?"

Standardmäßig verwenden wir einige der Bilder aus Ihren Paketen in den Store, wie in der Tabelle am oberen Rand dieser Seite beschrieben (zusammen mit anderen [Images, die Sie während der Übertragung bereitstellen](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Allerdings müssen Sie die Option aus, um zu verhindern, dass der Store die Logobilder in Ihrer app-Paketen verwenden, wenn Ihr Angebot für Kunden, die unter Windows 10 (einschließlich Xbox) angezeigt und müssen stattdessen den Store, die nur Bilder verwenden, die Sie hochladen. Dies bietet Ihnen mehr Kontrolle über die Darstellung Ihrer App in verschiedenen Anzeigen im Store. (Beachten Sie, wenn Ihr Produkt frühere Betriebssystemversionen unterstützt, die Kunden weiterhin-Images aus Ihrer Pakete angezeigt werden können, selbst wenn Sie diese Option verwenden.) Hierzu können Sie der **Store-Logos** Teil der **Store auflisten** Schritt des Übermittlungsprozesses.

![Angeben von Store-Logos während der app-Übermittlungsprozess](images/app-icons/storelogodisplay.png)

Wenn Sie dieses Kontrollkästchen aktivieren, ein neuer Abschnitt namens **Store Anzeigen von Bildern** angezeigt wird. Hier können Sie 3 Bildgrößen hochladen, die den Store anstelle Logobilder aus Ihrer app-Paketen verwendet werden: 71 x 71, 300 x 300 und 150 x 150 Pixel. Nur die 300 x 300-Größe ist erforderlich, jedoch empfohlen, alle 3 Größen bereitstellen.

Weitere Informationen finden Sie unter [Anzeige nur hochgeladen Logos in der Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Verwalten von app-Symbole mit dem Visual Studio-Manifest-Designer

Visual Studio bietet ein sehr nützliches Tool für die Verwaltung Ihrer app-Symbole wird aufgerufen, die **Manifest-Designer**. 

> Wenn Sie Visual Studio 2017 noch nicht, es gibt mehrere Versionen zur Verfügung, darunter eine kostenlose Version (Visual Studio 2017 Community Edition), und anderen Versionen bieten kostenlose Testversionen. Sie können diese hier herunterladen: [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


So starten Sie den Manifest-Designer:
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Verwenden Sie Visual Studio, um eine UWP-Projekt zu öffnen.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. In der **Projektmappen-Explorer**, doppelklicken Sie auf die Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Klicken Sie auf die **visuelle Anlagen** Registerkarte.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Zum Erstellen aller Objekte auf einmal

Das erste Menüelement der **visuelle Anlagen** Registerkarte **alle visuellen Assets**, ist genau das, was der Name schon sagt: alle Ihre app mit einem Klick muss visueller generiert.

![Generieren Sie alle visuellen Assets in Visual Studio](images/app-icons/all-visual-assets.png)

Ist alles, was Sie tun müssen ein einzelnes Abbild bereitzustellen, und Visual Studio generiert die Kachel "small", mittelgroße Kachel, große Kachel, Breite Kachel, große Kachel, app-Symbol, Splash-Bildschirm, und Paket-Logo-Ressourcen für jede Skalierungsfaktor.

Um alle Objekte auf einmal zu generieren:
1. Klicken Sie auf die **...**  neben der **Quelle** ein, und wählen Sie das Bild, das Sie verwenden möchten. Wenn Sie ein Bitmap-Bild verwenden, stellen Sie sicher, dass es mindestens 400, 400 Pixel ist, sodass Sie Spitze Ergebnisse erhalten. Vektorbasierte Bilder funktionieren am besten; Visual Studio können Sie künstliche Intelligenz (Adobe Illustrator) und PDF-Dateien verwenden. 
2. (Optional). In der **Anzeigeeinstellungen** Abschnitt, diese Optionen zu konfigurieren:

    a.  **Kurzname**:  Geben Sie einen kurzen Namen für Ihre app ein.

    b.  **Namen anzeigen**: Geben Sie an, ob es sich bei den kurzen Namen auf "Mittel", breit oder große Kacheln angezeigt werden soll. 

    c. **Kachel Hintergrund**: Geben Sie den hexadezimalen Wert oder einen Farbnamen für die Hintergrundfarbe der Kachel. Beispiel: `#464646`. Der Standardwert ist `transparent`.

    d. **Hintergrund: Splash**: Geben Sie den hexadezimalen Wert oder Farbe-Namen für den Hintergrund der: Splash-Bildschirm. 

3. Klicken Sie auf **generieren**. 

Visual Studio generiert die Bilddateien und Projekt hinzugefügt. Wenn Sie Ihre Ressourcen ändern möchten, wiederholen Sie einfach den Prozess. 

Skalierte Symbolressourcen verwenden die Dateibenennungskonvention:

*filename*-scale-*scale factor*.png

Beispiel:

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Beachten Sie, dass Visual Studio ein infoanzeigerlogo generiert standardmäßig nicht. Der Grund ist Ihre badgelogo ist eindeutig und sollte nicht stimmt nach Möglichkeit Ihre app-Symbole. Weitere Informationen finden Sie in der [Badge Benachrichtigungen für UWP-apps Artikel](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Weitere Informationen zu app-Symbolressourcen
Visual Studio generiert alle app-Symbol-Objekte, die für das Projekt erforderlich, aber wenn Sie sie anpassen möchten, ist es hilfreich, um zu verstehen, wie sie sich von anderen app-Ressourcen sind. 

Das app-Symbol-Objekt wird in viele Stellen angezeigt: der Windows-Taskleiste, die Vorgangsansicht, ALT + TAB und unten rechts auf der Start-Kacheln. Da das app-Symbol-Objekt an so viele Stellen angezeigt wird, hat Sie einige zusätzliche Dimensionierung und die Optionen für die anderen Objekte haben keine plating: "Ziel-Größe" Assets "und"unplated"Assets. 

### <a name="target-size-app-icon-assets"></a>Zielgröße app-Symbolressourcen
Zusätzlich zu den standardmäßigen Faktor Skalierungsgröße ("Square44x44Logo.scale-400.png") empfehlen wir Ihnen ebenfalls "Ziel-Größe"-Objekte erstellen. Wir bezeichnen diese Assets Zielgröße, da sie bestimmte Größen, wie z. B. 16 Pixeln anstatt mit bestimmten Skalierungsfaktoren, z. B. 400 ausgerichtet. Zielgröße Ressourcen sind für Flächen, die die Skalierung Plateau-System nicht verwenden:

* Starten der Sprungliste (Desktop)
* Starten der unteren Ecke der Kachel (Desktop)
* Tastenkombinationen (Desktop)
* Systemsteuerung (Desktop)

So sieht die Liste der Zielgröße Objekte aus:


| Ressourcengröße | Dateinamenbeispiel                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20 x 20      | Square44x44Logo.targetsize-20.png  |
| 30 x 30      | Square44x44Logo.targetsize-30.png  |
| 36 x 36      | Square44x44Logo.targetsize-36.png  |
| 40 x 40      | Square44x44Logo.targetsize-40.png  |
| 60 x 60      | Square44x44Logo.targetsize-60.png  |
| 64 x 64      | Square44x44Logo.targetsize-64.png  |
| 72 x 72      | Square44x44Logo.targetsize-72.png  |
| 80 x 80      | Square44x44Logo.targetsize-80.png  |
| 96 x 96      | Square44x44Logo.targetsize-96.png  |

\* Es wird empfohlen, mindestens diese Größen bereitstellen. 

Sie müssen für diese Ressourcen keine Abstände hinzufügen, diese werden bei Bedarf von Windows hinzugefügt. Bei diesen Ressourcen sollte eine minimale Fläche von 16 Pixeln vorgesehen werden. 

Unten sehen Sie ein Beispiel dafür, wie diese Ressourcen als Symbole in der Windows-Taskleiste angezeigt werden:

![Ressourcen in Windows-Taskleiste](images/assetguidance21.png)

### <a name="unplated-assets"></a>Unplated assets
Standardmäßig wird eine Ziel-basierten Ressource auf einer farbigen Bezeichnung Windows wird standardmäßig verwendet. Wenn Sie möchten, können Sie ein Ziel-basierte unplated Medienobjekt bereitstellen. "Unplated" bedeutet, dass es sich bei das Medienobjekt einen transparenten Hintergrund angezeigt wird. Bedenken Sie, die diese Objekte über eine Vielzahl von Hintergrundfarben angezeigt werden. 

![Ressourcen mit und ohne Anpassung](images/assetguidance22.png)

Hier sind die Flächen, die unplated app-Symbolressourcen verwenden:
* Taskleiste und Miniaturansicht der Taskleiste (Desktop)
* Taskleisten-Sprungliste
* Aufgabenansicht
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Ziel und unplated größenanpassung

Hier sind die empfohlenen Größen für Ziel-basierte Ressourcen bedarfsorientiert 100 % aus:

![Zielbasierte Ressourcengröße bei einer Skalierung von 100 %](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Weitere Informationen zu Ressourcen für Splash-Bildschirm
Weitere Informationen zu Begrüßungsbildschirme, finden Sie unter den [UWP Splash-Bildschirm Artikel](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Weitere Informationen zu den Badge-Logo-Ressourcen

Wenn Sie den Asset-Generator verwenden, um alle Objekte zu generieren, Sie müssen, es gibt ein Grund, warum es Logos für infoanzeiger standardmäßig generiert nicht: sie sind sehr stark von anderen app-Ressourcen. Die badgelogo ist ein Bild, das in Benachrichtigungen und auf der app-Kacheln angezeigt wird. 

Weitere Informationen finden Sie unter den [Badge Benachrichtigungen für UWP-apps Artikel](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Anpassen des Asset-Auffüllung

Standardmäßig wird mit Visual Studio-Asset-Generator beliebige Image empfohlen Auffüllung gilt. Wenn Ihre Images enthalten bereits die Auffüllung, oder Sie möchten vollständige Anschnitt-Images, die an das Ende der Kachel erweitern, Sie können diese Funktion deaktivieren durch Deaktivieren der **empfohlen Auffüllung anwenden** Kontrollkästchen. 

### <a name="tile-padding-recommendations"></a>Kachel "Padding-Empfehlungen
Wenn Sie eigene Auffüllung bereitstellen möchten, sind hier unsere Empfehlungen für Kacheln. 

Es gibt 4 kachelgrößen: Small (71 x 71), Mittel (150 x 150), Wide (310 x 150) und Groß (310 x 310). 

Jede Kachelressource hat die gleiche Größe wie die Kachel, auf der sie sich befindet.

![Kachel mit vollständigen bekannt gegeben.](images/app-icons/tile-assets1.png)

Wenn Sie nicht, dass Ihr Symbol am Rand der Kachel erweitern möchten, können transparente Pixel in Ihr Medienobjekt Sie zum Erstellen der Auffüllung. 

![Kachel und Rückwand](images/assetguidance05.png)

Beschränken Sie bei kleinen Kacheln die Breite und Höhe auf 66 % der Kachelgröße:

![Verhältnisse bei kleinen Kachelgrößen](images/assetguidance09.png)

Beschränken Sie bei mittelgroßen Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße. Dadurch wird verhindert, dass Elemente in der Brandingleiste überlappen:

![Verhältnisse bei mittelgroßen Kacheln](images/assetguidance10.png)

Beschränken Sie bei breiten Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße. Dadurch wird verhindert, dass Elemente in der Brandingleiste überlappen:

![Verhältnisse bei breiten Kacheln](images/assetguidance11.png)

Beschränken Sie bei großen Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße:

![Verhältnisse bei großen Kacheln](images/assetguidance12.png)

Einige Symbole sind so konzipiert, dass sie horizontal oder vertikal ausgerichtet sind, andere Symbole hingegen weisen komplexere Formen auf, wodurch verhindert wird, dass sie direkt in die Zielabmessungen passen. Symbole, die zentriert erscheinen, können auf eine Seite geneigt sein. In diesem Fall können Teile eines Symbols aus der empfohlenen Fläche heraushängen, vorausgesetzt, das Symbol belegt dasselbe visuelle Gewicht wie ein genau passendes Symbol:

![Drei zentrierte Symbole](images/assetguidance13.png)

Berücksichtigen Sie bei randlosen Ressourcen Elemente, die innerhalb der Ränder und Kanten der Kacheln interagieren. Behalten Sie Ränder von mindestens 16 % der Höhe oder Breite der Kachel bei. Dieser Prozentsatz stellt die doppelte Breite der Ränder bei den kleinsten Kachelgrößen dar:

![Randlose Kachel mit Rändern](images/assetguidance14.png)

In diesem Beispiel sind die Ränder zu eng:

![Randlose Kachel mit zu engen Rändern](images/assetguidance15.png)









