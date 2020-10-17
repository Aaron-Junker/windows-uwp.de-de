---
description: Erstellen von App-Symbolen/-Logos, die Ihre App im Startmenü, auf App-Kacheln, in der Taskleiste, im Microsoft Store und an anderen Orten darstellen.
title: App-Symbole und Logos
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4e908cbad1fb0b70fe96af50917e8b895fdda90d
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860122"
---
# <a name="app-icons-and-logos"></a>App-Symbole und Logos 

Jede App hat ein Symbol/Logo, das sie darstellt, und dieses Symbol wird an mehreren Orten in der Windows-Shell angezeigt: 

:::row:::
    :::column:::
        * In der App-Liste im Startmenü
        * In der Taskleiste und im Task-Manager
        * Auf den Kacheln Ihrer App
        * Auf dem Begrüßungsbildschirm Ihrer App
        * Im Microsoft Store
    :::column-end:::
    :::column:::
        ![Starten von Windows 10 und Kacheln](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Dieser Artikel behandelt die Grundlagen der Erstellung von App-Symbolen, wie Sie diese mit Visual Studio verwalten, und wie Sie sie im Bedarfsfall manuell verwalten.
 
(Dieser Artikel bezieht sich speziell auf Symbole, die die App selbst darstellen. Eine allgemeine Anleitung zu Symbolen finden Sie in dem Artikel [Symbole](icons.md).)

## <a name="icon-types-locations-and-scale-factors"></a>Symboltypen, Ort und Skalierungsfaktoren

Standardmäßig speichert Visual Studio Symbolressourcen in einem Unterverzeichnis für Ressourcen. Hier finden Sie eine Liste der verschiedenen Typen von Symbolen, wo diese vorkommen und wie sie heißen. 

| Symbolname | Kommt vor in | Ressourcendateiname |
| ---      | ---        | --- |
| Kleine Kachel | Startmenü |  SmallTile.png  |
| Mittelgroße Kachel |Startmenü, Microsoft Store-Eintrag\*  |  Square150x150Logo.png |
| Breite Kachel  | Startmenü   | Wide310x150Logo.png |
| Große Kachel   | Startmenü, Microsoft Store-Eintrag\* |  LargeTile.png  |
| App-Symbol | App-Liste im Startmenü, Taskleiste, Task-Manager | Square44x44Logo.png |
| Begrüßungsbildschirm | Begrüßungsbildschirm der App | SplashScreen.png  |
| Badgelogo | Auf den Kacheln Ihrer App | BadgeLogo.png  |
| Paketlogo/Store-Logo | App-Installationsprogramm, Partner Center, die Option „Eine App melden“ im Store, die Option „Eine Rezension schreiben“ im Store | StoreLogo.png  |

\* Wird verwendet, außer Sie wählen aus, dass [nur hochgeladene Bilder im Store angezeigt werden sollen](../../publish/app-screenshots-and-images.md#display-only-uploaded-logo-images-in-the-store). 

Um sicherzustellen, dass diese Symbole auf allen Bildschirmen scharf aussehen, können Sie mehrere Versionen desselben Symbols für unterschiedliche Skalierungsfaktoren von Anzeigegeräten erstellen. 

Der Skalierungsfaktor bestimmt die Größe von UI-Elementen wie Text. Skalierungsfaktoren liegen zwischen 100 % und 400 %. Größere Werte erstellen größere UI-Elemente, wodurch sie auf Anzeigegeräten mit hoher Auflösung (DPI) leichter zu sehen sind. 

:::row:::
   :::column:::
      Windows legt den Skalierungsfaktor für jede Anzeige automatisch aus, basierend auf dem DPI-Wert (Punkte pro Zoll) und dem Betrachtungsabstand des Geräts. 
      (Benutzer können den Standardwert außer Kraft setzen, indem sie zur Seite **Einstellungen &gt; Anzeige &gt; Skalierung und Anordnung** navigieren.)
   :::column-end:::
   :::column:::
      ![Screenshot der Seite „Anzeige“ in den Einstellungen.](images/icons/display-settings-screen.png)
   :::column-end:::
:::row-end:::  


Da App-Symbolressourcen Bitmaps sind und Bitmaps sich nicht gut skalieren lassen, empfehlen wir, eine Version jeder Symbolressource für jeden Skalierungsfaktor bereitzustellen: 100 %, 125 %, 150 %, 200 % und 400 %. Das sind eine Menge Symbole! Glücklicherweise bietet Visual Studio ein Tool, das das Generieren und Aktualisieren diese Symbole erleichtert. 

## <a name="microsoft-store-listing-image"></a>Bild im Microsoft Store-Eintrag

„Wie gebe ich Bilder für den Eintrag meiner App im Microsoft Store an?“

Standardmäßig verwenden wir einige der Bilder aus Ihren Paketen im Store, wie in der Tabelle am Anfang dieser Seite beschrieben (zusammen mit anderen [Bildern, die Sie während des Übermittlungsprozesses bereitstellen](../../publish/app-screenshots-and-images.md)). Sie haben jedoch die Möglichkeit, zu verhindern, dass der Store die Logobilder verwendet, die in den Paketen Ihrer App enthalten sind, wenn Ihr Eintrag für Kunden mit Windows 10 (einschließlich Xbox) angezeigt wird, und können den Store dazu zwingen, dass stattdessen nur Bilder verwendet werden, die Sie hochladen. Dies bietet Ihnen mehr Kontrolle über die Darstellung Ihrer App auf verschiedenen Anzeigegeräten im Store. (Beachten Sie, dass, wenn Ihr Produkt frühere Betriebssystemversionen unterstützt, diesen Kunden eventuell weiterhin Bilder aus Ihren Paketen angezeigt werden können, selbst wenn Sie diese Option verwenden.) Sie können dies im Abschnitt **Store-Logos** des Schritts **Store-Eintrag** des Übermittlungsprozesses erledigen.

![Angeben von Store-Logos während des App-Übermittlungsprozesses](images/app-icons/storelogodisplay.png)

Durch Aktivieren dieses Kontrollkästchens wird ein neuer Abschnitt namens **Bilder für die Store-Anzeige** angezeigt. Hier können sie 3 Bildgrößen hochladen, die vom Store anstelle von Logobildern aus den Paketen Ihrer App verwendet werden: 300 x 300, 150 x 150 und 71 x 71 Pixel. Nur die Größe 300 x 300 ist erforderlich, obwohl wir empfehlen, alle 3 Größen bereitzustellen.

Weitere Informationen finden Sie unter [Nur hochgeladene Logobilder im Store anzeigen](../../publish/app-screenshots-and-images.md#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](../../publish/app-screenshots-and-images.md). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Verwalten von App-Symbolen mit dem Visual Studio Manifest-Designer

Visual Studio bietet ein sehr nützliches Tool für die Verwaltung Ihrer App-Symbole namens **Manifest-Designer**. 

> Wenn Sie nicht schon Visual Studio 2019 besitzen, sind mehrere Versionen erhältlich, einschließlich einer kostenlosen Version (Visual Studio 2019 Community Edition), und die anderen Versionen bieten kostenlose Testversionen an. Diese können Sie hier herunterladen: [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


So starten Sie den Manifest-Designer
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2019 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2019 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Verwenden Sie Visual Studio, um ein UWP-Projekt zu öffnen.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei „Package.appxmanifest“.
    :::column-end:::
    :::column:::
        ![Der Manifest-Designer in Visual Studio 2019](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            In Visual Studio wird der Manifest-Designer angezeigt.
    :::column-end:::
    :::column:::
            ![Screenshot des Manifestdesigners mit der Registerkarte „Anwendung“.](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Klicken Sie auf die Registerkarte **Visuelle Assets**.
    :::column-end:::
    :::column:::
        ![Screenshot des Manifestdesigners mit der Registerkarte „Visuelle Assets“.](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Gleichzeitiges Generieren aller Ressourcen

Das erste Menüelement auf der Registerkarte **Visuelle Assets**, **Alle visuellen Assets**, macht genau das, was der Name schon sagt: Es generiert alle visuellen Assets, die Ihre App benötigt, mit nur einem Klick auf eine Schaltfläche.

![Generieren aller visuellen Assets in Visual Studio](images/app-icons/all-visual-assets.png)

Alles, was Sie tun müssen, ist, ein einzelnes Bild bereitzustellen, und Visual Studio generiert die Assets für die kleine Kachel, die mittelgroße Kachel, die große Kachel, die breite Kachel, das App-Symbol, den Begrüßungsbildschirm sowie die Paket-Logo-Assets für alle Skalierungsfaktoren.

So generieren Sie alle Ressourcen gleichzeitig
1. Klicken Sie auf die **...**  neben dem Feld **Quelle**, und wählen Sie das Bild aus, das Sie verwenden möchten. Wenn Sie ein Bitmap-Bild verwenden, stellen Sie sicher, dass es mindestens 400 x 400 Pixel groß ist, damit Sie ein scharfes Ergebnisse erhalten. Vektorbasierte Bilder funktionieren am besten. Visual Studio gestattet Ihnen die Verwendung von Adobe Illustrator (AI)- und PDF-Dateien. 
2. (Optional.) Konfigurieren Sie im Abschnitt **Anzeigeeinstellungen** diese Optionen:

    ein.  **Kurzname**:  Geben Sie einen Kurznamen für Ihre App an.

    b.  **Name anzeigen**: Geben Sie an, ob der Kurzname auf mittelgroßen, breiten oder großen Kacheln angezeigt werden soll. 

    c. **Kachelhintergrund**: Geben Sie den Hexadezimalwert oder einen Farbnamen für die Hintergrundfarbe der Kachel an. Beispiel: `#464646`. Der Standardwert ist `transparent`.

    d. **Hintergrund des Begrüßungsbildschirms**: Geben Sie den Hexadezimalwert oder Farbnamen für den Hintergrund des Begrüßungsbildschirms an. 

3. Klicken Sie auf **Generieren**. 

Visual Studio generiert Ihre Bilddateien und fügt sie dem Projekt hinzu. Wenn Sie Ihre Ressourcen ändern möchten, wiederholen Sie den Prozess einfach. 

Skalierte Symbolressourcen befolgen diese Dateibenennungskonvention:

*Dateiname*-scale-*Skalierungsfaktor*.png

Beispiel:

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Beachten Sie, dass Visual Studio das Badgelogo nicht standardmäßig generiert. Der Grund hierfür ist, dass Ihr Badgelogo einzigartig ist und nicht mit Ihren anderen App-Symbolen übereinstimmen sollte. Weitere Informationen finden Sie in dem Artikel [Signalbenachrichtigungen für Windows-Apps](../shell/tiles-and-notifications/badges.md). 


## <a name="more-about-app-icon-assets"></a>Weitere Informationen zu App-Symbolressourcen
Visual Studio generiert alle App-Symbolressourcen, die für Ihr Projekt erforderlich sind, aber wenn Sie sie anpassen möchten, ist es hilfreich, zu verstehen, wie sie sich von anderen App-Ressourcen unterscheiden. 

Die App-Symbolressource kommt an vielen Stellen vor: in der Windows-Taskleiste, in der Taskansicht, bei ALT+TAB und in der unteren rechten Ecke von Startkacheln. Da die App-Symbolressource an so vielen Stellen angezeigt wird, stehen für sie einige zusätzliche Größen- und Hintergrundquadratoptionen zur Verfügung, über die andere Ressourcen nicht verfügen: Ressourcen mit „Zielgröße“ und Ressourcen „ohne Hintergrundquadrat“. 

### <a name="target-size-app-icon-assets"></a>App-Symbolressourcen mit Zielgröße
Zusätzlich zu den Standard-Skalierungsfaktorgrößen („Square44x44Logo.scale-400.png“) empfehlen wir Ihnen ebenfalls, Ressourcen mit „Zielgröße“ zu erstellen. Wir bezeichnen diese Ressourcen mit „Zielgröße“, weil sie auf bestimmte Größen abzielen, wie z. B. 16 Pixel, anstatt auf bestimmte Skalierungsfaktoren, z. B. 400. Ressourcen mit Zielgröße sind für Oberflächen gedacht, die nicht das Skalierungsplateau-System verwenden:

* Starten der Sprungliste (Desktop)
* Starten der unteren Ecke der Kachel (Desktop)
* Tastenkombinationen (Desktop)
* Systemsteuerung (Desktop)

Dies ist die Liste der Ressourcen in Zielgrößen:


| Ressourcengröße | Beispiel für Dateinamen                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20 x 20      | Square44x44Logo.targetsize-20.png  |
| 30 x 30      | Square44x44Logo.targetsize-30.png  |
| 36 x 36      | Square44x44Logo.targetsize-36.png  |
| 40 x 40      | Square44x44Logo.targetsize-40.png  |
| 60 x 60      | Square44x44Logo.targetsize-60.png  |
| 64 x 64      | Square44x44Logo.targetsize-64.png  |
| 72 x 72      | Square44x44Logo.targetsize-72.png  |
| 80 x 80      | Square44x44Logo.targetsize-80.png  |
| 96 x 96      | Square44x44Logo.targetsize-96.png  |

\* Wir empfehlen, mindestens diese Größen bereitzustellen. 

Sie müssen für diese Ressourcen keine Abstände hinzufügen, diese werden bei Bedarf von Windows hinzugefügt. Bei diesen Ressourcen sollte eine minimale Fläche von 16 Pixeln vorgesehen werden. 

Unten sehen Sie ein Beispiel dafür, wie diese Ressourcen als Symbole in der Windows-Taskleiste angezeigt werden:

![Ressourcen in Windows-Taskleiste](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ressourcen ohne Hintergrundquadrat
Standardmäßig verwendet Windows eine zielbasierte Ressource vor einem farbigen Hintergrundquadrat. Wenn Sie möchten, können Sie eine zielbasierte Ressource ohne Hintergrundquadrat bereitstellen. „Ohne Hintergrundquadrat“ bedeutet, dass die Ressource vor einem transparenten Hintergrund angezeigt wird. Denken Sie daran, dass diese Ressourcen vor einer Reihe verschiedener Hintergrundfarben angezeigt werden. 

![Ressourcen mit und ohne Anpassung](images/assetguidance22.png)

Dies sind die Oberflächen, die App-Symbolressourcen ohne Hintergrundquadrat verwenden:
* Taskleiste und Miniaturansicht der Taskleiste (Desktop)
* Taskleisten-Sprungliste
* Aufgabenansicht
* ALT+TAB

### <a name="unplated-assets-and-themes"></a>Ressourcen ohne Hintergrundquadrat und Designs

Das ausgewählte Design des Benutzers bestimmt die Farbe der Taskleiste. Wenn die Ressource ohne Hintergrundquadrat nicht speziell für das aktuelle Design qualifiziert ist, überprüft das System die Ressource hinsichtlich des Kontrasts. Wenn sie über ausreichend Kontrast zur Taskleiste verfügt, verwendet das System die Ressource. Andernfalls sucht das System nach einer Version der Ressource mit hohem Kontrast. Wenn keine gefunden wird, zeichnet das System stattdessen die Form der Ressource mit Hintergrundquadrat. 


### <a name="target-and-unplated-sizing"></a>Zielgröße und Größenanpassung ohne Hintergrundquadrat

Nachfolgend finden Sie die Größenempfehlungen für zielbasierte Ressourcen mit einer Skalierung von 100 %:

![Zielbasierte Ressourcengröße bei einer Skalierung von 100 %](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Weitere Informationen zu Ressourcen für den Begrüßungsbildschirm
Weitere Informationen zu Begrüßungsbildschirmen finden Sie unter [Begrüßungsbildschirme für Windows-Apps](../../launch-resume/splash-screens.md).

## <a name="more-about-badge-logo-assets"></a>Weitere Informationen zu Badgelogoressourcen

Wenn Sie den Asset-Generator verwenden, um alle benötigten Ressourcen zu generieren, gibt es einen Grund, warum er Badgelogos nicht standardmäßig generiert: Sie unterscheiden sich sehr stark von anderen App-Ressourcen. Das Badgelogo ist ein Statusbild, das in Benachrichtigungen und auf den Kacheln der App angezeigt wird. 

Weitere Informationen finden Sie unter [Signalbenachrichtigungen für Windows-Apps](../shell/tiles-and-notifications/badges.md).


## <a name="customizing-asset-padding"></a>Anpassen der Abstandsauffüllung von Ressourcen

Standardmäßig wendet der Asset-Generator von Visual Studio die empfohlene Abstandsauffüllung auf jedes Bild an. Wenn Ihre Bilder bereits Abstandsauffüllung enthalten, oder wenn Sie randlose Bilder möchten, die bis zum Rand der Kachel gehen, können Sie diese Funktion deaktivieren, indem Sie das Kontrollkästchen **Empfohlene Auffüllung anwenden** deaktivieren. 

### <a name="tile-padding-recommendations"></a>Empfehlungen für Abstandsauffüllung von Kacheln
Wenn Sie Ihre eigene Abstandsauffüllung bereitstellen möchten, finden Sie hier unsere Empfehlungen für Kacheln. 

Es gibt 4 Kachelgrößen: Klein (71 x 71), Mittel (150 x 150), Breit (310 x 150) und Groß (310 x 310). 

Jede Kachelressource hat die gleiche Größe wie die Kachel, auf der sie sich befindet.

![Randlose Kachel](images/app-icons/tile-assets1.png)

Wenn Sie nicht möchten, dass Ihr Symbol bis zum Rand der Kachel geht, können Sie transparente Pixel in Ihrer Ressource verwenden, um Abstandsauffüllung herzustellen. 

![Kachel und Rückwand](images/assetguidance05.png)

Beschränken Sie bei kleinen Kacheln die Breite und Höhe auf 66 % der Kachelgröße:

![Verhältnisse bei kleinen Kachelgrößen](images/assetguidance09.png)

Beschränken Sie bei mittelgroßen Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße. Dadurch wird verhindert, dass Elemente in der Brandingleiste überlappen:

![Verhältnisse bei mittelgroßen Kacheln](images/assetguidance10.png)

Beschränken Sie bei breiten Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße. Dadurch wird verhindert, dass Elemente in der Brandingleiste überlappen:

![Verhältnisse bei breiten Kacheln](images/assetguidance11.png)

Beschränken Sie bei großen Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße:

![Verhältnisse bei großen Kacheln](images/assetguidance12.png)

Einige Symbole sind so konzipiert, dass sie horizontal oder vertikal ausgerichtet sind, andere Symbole hingegen weisen komplexere Formen auf, wodurch verhindert wird, dass sie direkt in die Zielabmessungen passen. Symbole, die zentriert erscheinen, können auf eine Seite geneigt sein. In diesem Fall können Teile eines Symbols aus der empfohlenen Fläche heraushängen, vorausgesetzt, das Symbol belegt dasselbe visuelle Gewicht wie ein genau passendes Symbol:

![Drei zentrierte Symbole](images/assetguidance13.png)

Berücksichtigen Sie bei randlosen Ressourcen Elemente, die innerhalb der Ränder und Kanten der Kacheln interagieren. Behalten Sie Ränder von mindestens 16 % der Höhe oder Breite der Kachel bei. Dieser Prozentsatz stellt die doppelte Breite der Ränder bei den kleinsten Kachelgrößen dar:

![Randlose Kachel mit Rändern](images/assetguidance14.png)

In diesem Beispiel sind die Ränder zu eng:

![Randlose Kachel mit zu engen Rändern](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>Optimieren für bestimmte Designs, Sprachen und andere Bedingungen 

In diesem Artikel wurde beschrieben, wie Sie Ressourcen für bestimmte Skalierungsfaktoren erstellen, aber Sie können auch Ressourcen für eine Vielzahl verschiedener Bedingungen und Kombinationen von Bedingungen erstellen. Beispielsweise können Sie Symbole für Anzeigegeräte mit hohem Kontrast oder für die hellen und dunklen Designs erstellen. Sie können sogar Ressourcen für bestimmte Sprachen erstellen.

Anleitungen hierzu finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und andere Eigenschaften](../../app-resources/tailor-resources-lang-scale-contrast.md).
