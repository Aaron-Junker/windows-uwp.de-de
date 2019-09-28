---
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: Optimieren von Animationen, Medien und Bildern
description: Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 579772bba55c93de38c3c43538ad14253dbc2572
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339899"
---
# <a name="optimize-animations-media-and-images"></a>Optimieren von Animationen, Medien und Bildern


Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe.

## <a name="make-animations-smooth"></a>Reibungsloses Einbinden von Animationen

Ein wesentlicher Aspekt der UWP-Apps sind reibungslose Interaktionen. Dazu zählen Fingereingabemanipulationen, die sozusagen am Finger „kleben“, flüssige Übergänge und Animationen sowie kleine Bewegungen, die Eingabefeedback liefern. Das XAML-Framework verfügt über einen so genannten Kompositionsthread, der für die Zusammenstellung und Animation der visuellen Elemente einer App vorgesehen ist. Da der Kompositionsthread vom UI-Thread (dem Thread, der den Framework- und den Entwicklercode ausführt) getrennt ist, können Apps unabhängig von komplizierten Layoutdurchläufen oder erweiterten Berechnungen eine gleichmäßige Bildfrequenz und flüssige Animationen erzielen. In diesem Abschnitt erfahren Sie, wie Sie mithilfe des Kompositionsthreads butterweiche App-Animationen auf den Bildschirm zaubern. Weitere Informationen zu Animationen finden Sie unter [Übersicht über Animationen](https://docs.microsoft.com/windows/uwp/graphics/animations-overview). Informationen zum Verbessern der Reaktionsfähigkeit bei rechenintensiven Vorgängen finden Sie unter [Sicherstellen der Reaktionsfähigkeit des UI-Threads](keep-the-ui-thread-responsive.md).

### <a name="use-independent-instead-of-dependent-animations"></a>Verwenden unabhängiger Animationen anstelle von abhängigen Animationen

Unabhängige Animationen können zum Zeitpunkt der Erstellung von Anfang bis Ende berechnet werden, da sich Änderungen an der animierten Eigenschaft nicht auf die restlichen Objekte einer Szene auswirken. Unabhängige Animationen können daher im Kompositionsthread und nicht im UI-Thread ausgeführt werden. Dadurch ist gewährleistet, dass die Animationen flüssig verlaufen, denn der Kompositionsthread wird in einem gleichmäßigen Rhythmus aktualisiert.

Alle diese Animationsarten sind garantiert unabhängig:

-   Objektanimationen mit Keyframes
-   Animationen ohne Dauer
-   Animationen der Eigenschaften [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) und [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top)
-   Animationen der [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft
-   Animationen von Eigenschaften vom Typ [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush), wenn auf die untergeordnete Eigenschaft [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) abgezielt wird
-   Animationen der folgenden [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)-Eigenschaften, wenn auf untergeordneten Eigenschaften dieser Rückgabewerttypen abgezielt wird:

    -   [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
    -   [**Projektion**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection)
    -   [**Techno**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)

Abhängige Animationen wirken sich auf das Layout aus und können nicht ohne zusätzliche Angaben aus dem UI-Thread berechnet werden. Abhängige Animationen umfassen Änderungen an Eigenschaften wie [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) und [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height). Abhängige Animationen werden standardmäßig nicht ausgeführt und erfordern die Zustimmung des App-Entwicklers. Werden sie aktiviert, laufen sie flüssig ab, solange der UI-Thread nicht blockiert wird. Ist das Framework oder die App dagegen stark mit anderen Vorgängen des UI-Threads ausgelastet, beginnen die Animationen zu ruckeln.

Nahezu alle Animationen im XAML-Framework sind standardmäßig unabhängig. Sie können diese Optimierung jedoch mithilfe einiger Schritte deaktivieren. Vorsicht ist besonders bei folgenden Szenarien geboten:

-   Beim Festlegen der [**EnableDependentAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.pointanimation.enabledependentanimation)-Eigenschaft, um die Ausführung einer abhängigen Animation im UI-Thread zu ermöglichen. Beim Konvertieren dieser Animationen in eine unabhängige Version. Animieren Sie beispielsweise [**ScaleTransform.ScaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) und [**ScaleTransform.ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaley) anstelle der Eigenschaften [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) und [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) eines Objekts. Objekte wie Bilder und Text können ohne Weiteres skaliert werden. Das Framework wendet die bilineare Skalierung nur während der Animierung von [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform) an. Bilder/Texte werden in der endgültigen Größe neu gerastert, um zu gewährleisten, dass sie stets gut erkennbar sind.
-   Bei Verwendung framespezifischer Updates. Hierbei handelt es sich de facto um abhängige Animationen. Ein Beispiel hierfür ist das Anwenden von Transformationen im Handler des [**CompositonTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering)-Ereignisses.
-   Beim Ausführen einer beliebigen, als unabhängig geltenden Animation in einem Element, bei dem die [**CacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.cachemode)-Eigenschaft auf **BitmapCache** festgelegt ist. Dies wird als abhängige Animation betrachtet, da der Cache für jeden Frame neu gerastert werden muss.

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>Animieren Sie keine WebView- oder MediaPlayerElement-Objekte.

Webinhalte innerhalb eines [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)-Steuerelements werden nicht direkt vom XAML-Framework gerendert. Zudem ist das Zusammenfügen mit dem Rest der Szene mit zusätzlichem Aufwand verbunden. Dieser Zusatzaufwand erhöht sich, wenn das animierte Steuerelement über den Bildschirm bewegt wird. Dies kann unter Umständen zu Synchronisierungsproblemen führen (der HTML-Inhalt könnte z. B. nicht synchron mit dem restlichen XAML-Inhalt der Seite sein). Wenn Sie ein **WebView**-Steuerelement animieren müssen, ersetzen Sie es für die Dauer der Animation durch ein [**WebViewBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush)-Objekt.

Auch das Animieren eines [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)-Objekts ist nicht empfehlenswert. Abgesehen von den Leistungsbeeinträchtigungen kann dies im wiedergegebenen Videoinhalt zu Bildstörungen und anderen Artefakten führen.

> **Hinweis**   die Empfehlungen in diesem Artikel für **mediaplayerelement** gelten auch für [**Media Element**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). **MediaPlayerElement** ist nur in Windows 10, Version 1607 verfügbar. Wenn Sie daher eine App für eine frühere Version von Windows verwenden, müssen Sie **MediaElement** verwenden.

### <a name="use-infinite-animations-sparingly"></a>Setzen Sie Endlosanimationen sparsam ein.

Die meisten Animationen werden für einen bestimmten Zeitraum ausgeführt. Wenn Sie die [**Timeline.Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Eigenschaft jedoch auf „Forever“ festlegen, kann die Animation ohne zeitliche Begrenzung ausgeführt werden. Wir empfehlen, Endlosanimationen möglichst sparsam einzusetzen, da solche Animationen fortlaufend CPU-Ressourcen verbrauchen. Dadurch kann die CPU unter Umständen nicht in den Stromsparmodus oder in den Leerlauf wechseln, was wiederum einen höheren Energieverbrauch zur Folge hat.

Das Hinzufügen eines Handlers für [**CompositionTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) ist vergleichbar mit dem Ausführen eines Endlosanimation. Normalerweise ist der UI-Thread nur aktiv, wenn es etwas für ihn zu tun gibt. Fügen Sie jedoch einen Handler für dieses Ereignis hinzu, muss er in jedem Frame ausgeführt werden. Entfernen Sie den Handler, wenn es nichts zu tun gibt, und registrieren Sie ihn erneut, wenn er wieder benötigt wird.

### <a name="use-the-animation-library"></a>Nutzen Sie die Animationsbibliothek.

Der [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation)-Namespace umfasst eine Bibliothek mit schnellen Hochleistungsanimationen, deren Erscheinungsbild mit anderen Windows-Animation konsistent ist. Die relevanten Klassen enthalten „Design“ in ihrem Namen und werden in [Übersicht über Animationen](https://docs.microsoft.com/windows/uwp/graphics/animations-overview) beschrieben. Diese Bibliothek unterstützt viele gängige Animationsszenarien, z. B. die Animation der ersten Ansicht einer App oder die Erstellung von Zustands- und Inhaltsübergängen. Wir empfehlen, die Animationsbibliothek so oft wie möglich zu nutzen, um die Leistung und Konsistenz für UWP-UI zu verbessern.

> **Hinweis**   die Animations Bibliothek kann nicht alle möglichen Eigenschaften animieren. Informationen zu XAML-Szenarien, bei denen die Animationsbibliothek nicht verwendet werden kann, finden Sie unter [Storyboardanimationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).


### <a name="animate-compositetransform3d-properties-independently"></a>Eigenständiges Animieren von CompositeTransform3D-Eigenschaften

Sie können jede [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.CompositeTransform3D)-Eigenschaft eigenständig animieren. Wenden Sie daher nur die Animationen an, die Sie benötigen. Beispiele und weitere Informationen finden Sie unter [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d). Weitere Informationen zum Animieren von Transformationen finden Sie unter [Storyboardanimationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations) und [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](https://docs.microsoft.com/windows/uwp/graphics/key-frame-and-easing-function-animations).

## <a name="optimize-media-resources"></a>Optimieren von Medienressourcen

Audio, Video und Bilder sind verlockende Arten von Inhalten, die in den meisten Apps zum Einsatz kommen. Da immer mehr Medieninhalte aufgezeichnet und anstelle von SD-Qualität immer häufiger HD-Qualität verwendet wird, steigt der Ressourcenbedarf für die Speicherung, Decodierung und Wiedergabe dieser Inhalte. Das XAML-Framework nutzt die neuesten Features der UWP-Medienmodule, sodass Apps kostenlos von diesen Verbesserungen profitieren. Hier werden einige zusätzliche Tricks erläutert, mit denen Sie Medien in Ihrer UWP-App optimal nutzen können.

### <a name="release-media-streams"></a>Freigeben von Mediendatenströmen

Mediendateien sind die gängigsten und aufwendigsten Ressourcen, die von Apps genutzt werden. Da Mediendateien den Speicherbedarf Ihrer App beträchtlich steigern können, ist es wichtig, das Handle für die Medien unverzüglich freizugeben, sobald die App sie nicht mehr verwendet.

Wenn Ihre App also beispielsweise ein [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream)- oder [**IInputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IInputStream)-Objekt verwendet, müssen Sie die close-Methode für das Objekt aufrufen, sobald die App es nicht mehr verwendet. Dadurch wird das zugrundeliegende Objekt freigegeben.

### <a name="display-full-screen-video-playback-when-possible"></a>Anzeigen der Videowiedergabe im Vollbildmodus (sofern möglich)

Verwenden Sie in UWP-Apps stets die [**IsFullWindow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow)-Eigenschaft für das [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement), um das vollständige Rendern des Fensters zu aktivieren bzw. zu deaktivieren. Dies stellt sicher, dass während der Medienwiedergabe Optimierungen auf Systemebene genutzt werden.

Das XAML-Framework kann die Darstellung von Videoinhalten optimieren, wenn nur das Video gerendert wird. Dies senkt den Energieverbrauch und erhöht die Bildfrequenz. Um eine möglichst effiziente Medienwiedergabe zu erhalten, legen Sie die Größe eines **MediaPlayerElement**-Objekts auf die Breite und Höhe des Bildschirms fest. Zeigen Sie außerdem keine weiteren XAML-Elemente an.

Es gibt durchaus berechtigte Gründe für die Überlagerung von XAML-Elementen auf einem **MediaPlayerElement**-Objekt, das die gesamte Höhe und Breite des Bildschirms einnimmt, beispielsweise für Untertitel oder kurzzeitig angezeigte Transportsteuerelemente. Stellen Sie sicher, dass diese Elemente ausgeblendet werden (durch Festlegen von `Visibility="Collapsed"`), wenn sie nicht benötigt werden, um die Medienwiedergabe wieder in den effizientesten Zustand zu versetzen.

### <a name="display-deactivation-and-conserving-power"></a>Deaktivieren des Displays und Einsparen von Energie

Um ein Deaktivieren des Displays zu verhindern, wenn keine Benutzeraktion mehr festgestellt werden kann (z. B. bei der Wiedergabe eines Videos in einer App), können Sie [**DisplayRequest.RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) aufrufen.

Um Energie zu sparen und den Akku zu schonen, wird empfohlen, [**DisplayRequest.RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) aufzurufen und die Displayanforderung freizugeben, sobald diese nicht mehr benötigt wird.

Unten sind einige Situationen aufgeführt, in denen Sie die Displayanforderung freigeben sollten:

-   Die Videowiedergabe wird angehalten, z. B. per Benutzeraktion, wird gepuffert oder aufgrund begrenzter Bandbreite angepasst.
-   Die Wiedergabe wird gestoppt. Beispielsweise ist die Wiedergabe des Videos beendet oder die Darstellung vorüber.
-   Ein Wiedergabefehler ist aufgetreten. Es können beispielsweise Probleme mit der Netzwerkverbindung bestehen, oder eine Datei kann beschädigt sein.

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>Platzieren anderer Elemente neben dem eingebetteten Video

Apps bieten häufig eine eingebettete Ansicht, bei der das Video innerhalb einer Seite wiedergegeben wird. Es ist nun keine Vollbildoptimierung mehr verfügbar, da das [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) nicht die gesamte Seitengröße einnimmt und außerdem auch andere XAML-Objekte gezeichnet werden. Achten Sie darauf, nicht versehentlich in diesen Modus zu wechseln, indem Sie einen Rahmen um ein **MediaPlayerElement**-Objekt zeichnen.

Zeichnen Sie keine XAML-Elemente im Vordergrund des Videos, wenn sich dieses im eingebetteten Modus befindet. Andernfalls bedeutet dies einen Zusatzaufwand beim Erstellen der Szene. Ein gutes Optimierungsbeispiel für diese Situation ist das Platzieren von Transportsteuerelementen unterhalb eines eingebetteten Medienelements (anstatt im Vordergrund des Videos). In diesem Bild steht der rote Balken für eine Gruppe von Transportsteuerelementen (Wiedergabe, Pause, Stopp usw.).

![MediaPlayerElement mit überlagerten Elementen](images/videowithoverlay.png)

Platzieren Sie diese Steuerelemente nicht im Vordergrund eines Mediums, das sich nicht im Vollbildmodus befindet. Positionieren Sie die Transportsteuerelemente stattdessen irgendwo außerhalb des Bereichs, in dem das Medium gerendert wird. Im nächsten Bild befinden sich die Steuerelemente unterhalb des Mediums.

![MediaPlayerElement mit benachbarten Elementen](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>Verzögern des Festlegens der Quelle für ein MediaPlayerElement

Medienmodule sind aufwendige Objekte, und das XAML-Framework verzögert das Laden von DLLs sowie das Erstellen großer Objekte so lange wie möglich. Das [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) ist gezwungen, diese Arbeit auszuführen, nachdem dessen Quelle über die Eigenschaft [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) festgelegt wurde. Indem Sie dies festlegen, wenn der Benutzer wirklich ein Medium wiedergeben möchte, verzögern Sie den Großteil des Aufwands für das **MediaPlayerElement**-Objekt so weit wie möglich.

### <a name="set-mediaplayerelementpostersource"></a>Festlegen von MediaElement.PosterSource

Durch Festlegen von [**MediaPlayerElement.PosterSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource) kann XAML einige GPU-Ressourcen freigeben, die andernfalls verwendet würden. Diese API ermöglicht es einer App, so wenig Arbeitsspeicher wie möglich zu beanspruchen.

### <a name="improve-media-scrubbing"></a>Verbessern des Medien-Scrubbings

Bei Medienplattformen ist es immer eine besondere Herausforderung, dafür zu sorgen, dass das Scrubbing möglichst gut reagiert. Üblicherweise wird hierzu der Wert eines Schiebereglers geändert. Im Folgenden finden Sie einige Tipps für eine möglichst effiziente Verwendung:

-   Aktualisieren Sie den Wert des [**Schiebereglers**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) basierend auf einem Timer, der die [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) auf [**MediaPlayerElement.MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) abfragt. Stellen Sie sicher, dass Sie eine geeignete Aktualisierungsfrequenz für den Timer verwenden. Die Eigenschaft **Position** wird während der Wiedergabe nur alle 250 Millisekunden aktualisiert.
-   Die Größe der Schrittfrequenz für den Schieberegler muss gemäß der Videolänge skaliert werden.
-   Abonnieren Sie die Ereignisse [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) und [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) für den Schieberegler, um die [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate)-Eigenschaft auf „0“ festzulegen, wenn der Benutzer den Ziehpunkt des Schiebereglers bewegt.
-   Legen Sie die Medienposition im [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)-Ereignishandler manuell auf den Wert der Schiebereglerposition fest, um beim Scrubbing eine optimale Ziehpunktausrichtung zu gewährleisten.

### <a name="match-video-resolution-to-device-resolution"></a>Anpassen der Videoauflösung an die Geräteauflösung

Die Videodecodierung benötigt sehr viel Arbeitsspeicher sowie sehr viele GPU-Zyklen. Entscheiden Sie sich daher für ein Videoformat, das der Auflösung, mit der das Video wiedergegeben wird, am nächsten kommt. Es bringt nichts, die Ressourcen zur Decodierung eines 1080-Videos zu verwenden, wenn es ohnehin auf eine deutlich geringere Größe skaliert wird. Bei vielen Apps liegt ein Video nicht in mehreren Auflösungen vor. Ist dies jedoch der Fall, verwenden Sie möglichst eine Codierung, die der Anzeigeauflösung am nächsten kommt.

### <a name="choose-recommended-formats"></a>Wählen empfohlener Formate

Die Wahl des Medienformats kann ein sensibles Thema sein und ist häufig abhängig von geschäftspolitischen Entscheidungen. Im Hinblick auf die Leistung von UWP empfehlen wir H.264-Video als primäres Videoformat sowie AAC und MP3 als bevorzugte Audioformate. Bei der lokalen Dateiwiedergabe ist MP4 der bevorzugte Dateicontainer für Videoinhalte. Die Decodierung von H.264 wird von den meisten aktuellen Grafikkarten beschleunigt. Zudem gilt es zu berücksichtigen, dass die Beschleunigung bei vielen der auf dem Markt erhältlichen Grafikkarten häufig auf eine partielle Beschleunigung (oder auf die IDCT-Ebene) beschränkt ist und keine Hardwareauslagerung für die gesamte Datenstromebene (VLD-Modus) stattfindet, obwohl die Hardwarebeschleunigung für die VC-1-Decodierung weitgehend verfügbar ist.

Wenn die Kontrolle über die Videoinhaltsgenerierung vollständig bei Ihnen liegt, müssen Sie ein möglichst ausgeglichenes Verhältnis zwischen Komprimierungseffizienz und GOP-Struktur finden. Eine relativ geringe GOP-Größe bei B-Bildern kann die Leistung bei Such- oder Trickmodi verbessern.

Verwenden Sie für kurze Audioeffekte mit geringer Latenz (beispielsweise in Spielen) WAV-Dateien mit nicht komprimierten PCM-Daten, um den für komprimierte Audioformate typischen Verarbeitungsmehraufwand zu reduzieren.


## <a name="optimize-image-resources"></a>Optimieren von Bildressourcen

### <a name="scale-images-to-the-appropriate-size"></a>Skalieren von Bildern auf eine angemessene Größe

Bilder werden in sehr hohen Auflösungen erfasst, was dazu führen kann, dass Apps mehr CPU beim Decodieren der Bilddaten und mehr Arbeitsspeicher nach dem Laden vom Datenträger belegen. Es ist jedoch nicht sehr sinnvoll, ein Bild in hoher Auflösung zu decodieren und im Arbeitsspeicher zu speichern, nur um es dann kleiner als in seiner ursprünglichen Größe anzuzeigen. Erstellen Sie stattdessen eine Version des Bilds in der genauen Größe, in der es auf dem Bildschirm gezeichnet wird. Verwenden Sie hierzu die Eigenschaften [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

Nicht empfohlen:

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

Empfohlene Vorgehensweise:

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

Als Einheiten für [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) werden standardmäßig physische Pixel verwendet. Mit der [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype)-Eigenschaft kann dieses Verhalten geändert werden: Durch Festlegen von **DecodePixelType** auf **Logical** wird bei der Decodierungsgröße automatisch der aktuelle Skalierungsfaktor des Systems ähnlich wie andere XAML-Inhalte berücksichtigt. Daher ist es in der Regel sinnvoll, **DecodePixelType** auf **Logical** festzulegen, wenn beispielsweise **DecodePixelWidth** und **DecodePixelHeight** den Eigenschaften für Höhe und Breite des Bildsteuerelements entsprechen sollen, in dem das Bild angezeigt wird. Beim Standardverhalten mit Verwendung von physischen Pixeln müssen Sie selbst den aktuellen Skalierungsfaktor des Systems berücksichtigen; für den Fall, dass der Benutzer seine Anzeigeeinstellungen ändert, sollten Sie auf Benachrichtigungen über Skalierungsänderungen achten.

Wenn „DecodePixelWidth/Height“ explizit größer als die Bildanzeige auf dem Bildschirm festgelegt sind, belegt die App unnötigen zusätzlichen Arbeitsspeicher (bis zu 4 Byte pro Pixel), was bei großen Bildern deutlich ins Gewicht fällt. Zudem wird das Bild mit bilinearer Skalierung verkleinert und kann bei großen Skalierungsfaktoren unscharf dargestellt werden.

Wenn DecodePixelWidth/DecodePixelHeight explizit kleiner als die Bilddarstellung auf dem Bildschirm festgelegt werden, wird das Bild vergrößert und kann pixelig aussehen.

In einigen Fällen, in denen es nicht möglich ist, die passende Decodierungsgröße im Voraus zu bestimmen, sollten Sie die automatische Decodierung von XAML auf die richtige Größe nutzen. Dabei wird nach Möglichkeit versucht, das Bild in der passenden Größe zu decodieren, wenn keine explizite DecodePixelWidth/DecodePixelHeight angegeben ist.

Es wird empfohlen, eine explizite Decodierungsgröße festzulegen, wenn Sie die Größe des Bildinhalts bereits vorab kennen. Legen Sie gleichzeitig [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) auf **Logical** fest, wenn die angegebene Decodierungsgröße in Bezug auf andere XAML-Elementgrößen relativ ist. Wenn Sie z. B. die Inhaltsgröße explizit mit Image.Width und Image.Height festlegen, legen Sie DecodePixelType auf DecodePixelType.Logical fest, um die gleichen logischen Pixeldimensionen wie ein Bildsteuerelement zu verwenden, und verwenden sie dann explizit BitmapImage.DecodePixelWidth und/oder BitmapImage.DecodePixelHeight, um die Größe des Bilds zu steuern und potenziell mehr Arbeitsspeicher einzusparen.

Beachten Sie, dass Image.Stretch beim Bestimmen der Größe des decodierten Inhalts berücksichtigt werden sollte.

### <a name="right-sized-decoding"></a>Richtige Größe der Decodierung

Wenn Sie keine explizite Decodierungsgröße festlegen, versucht XAML, möglich viel Arbeitsspeicher zu sparen. Dabei wird das Bild auf die genaue Größe decodiert, in der es gemäß dem anfänglichen Layout der Seite angezeigt wird. Es wird empfohlen, die Anwendung so zu schreiben, dass dieses Feature nach Möglichkeit genutzt werden kann. Dieses Feature wird deaktiviert, wenn eine der folgenden Bedingungen erfüllt ist.

-   Das [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Objekt wird mit der Live-XAML-Struktur verbunden, nachdem der Inhalt mit [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) oder [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) festgelegt wurde.
-   Das Bild wird mit synchroner Decodierung wie [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) decodiert.
-   Das Bild wird durch Festlegung von [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) auf 0 oder [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) auf **Collapsed** für das Hostbildelement, den Pinsel oder ein beliebiges übergeordnetes Element ausgeblendet.
-   Das Bildsteuerelement bzw. der Pinsel verwendet für die [**Stretch**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch)-Aufzählung den Wert **None**.
-   Das Bild wird als [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid) verwendet.
-   `CacheMode="BitmapCache"` wird auf das Bildelement oder ein beliebiges übergeordnetes Element festgelegt.
-   Der Bildpinsel ist nicht rechteckig (z. B. bei Anwendung auf eine Form oder auf Text).

In den oben genannten Szenarien ist das Festlegen einer expliziten Decodierungsgröße die einzige Möglichkeit, um Arbeitsspeichereinsparungen zu erzielen.

Fügen Sie stets ein [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Objekt an die Live-Struktur an, bevor Sie die Quelle festlegen. Dies erfolgt jedes Mal automatisch, wenn ein Bildelement oder ein Pinsel im Markupcode angegeben wird. Beispiele finden Sie weiter unten im Abschnitt „Live-Struktur-Beispiele“. Vermeiden Sie beim Festlegen einer Datenstromquelle stets die Verwendung von [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource), und verwenden Sie stattdessen [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync). Zudem ist es sinnvoll, das Ausblenden von Bildinhalten (entweder durch eine Deckkraft von null oder die Sichtbarkeitseinstellung „collapsed“) zu vermeiden, während Sie darauf warten, dass das [**ImageOpened**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened)-Ereignis ausgelöst wird. Wägen Sie Ihre Entscheidung ab. Sie können in diesem Fall die Vorteile der automatischen Decodierung in der richtigen Größe nicht nutzen. Wenn Ihre App Bilder anfänglich ausblenden muss, sollte nach Möglichkeit auch die Decodierungsgröße explizit festgelegt werden.

**Livebaum-Beispiele**

Beispiel 1 (gut): URI (Uniform Resource Identifier) in Markup angegeben.

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Beispiel 2 – Markup: URI in CodeBehind angegeben.

```xaml
<Image x:Name="myImage"/>
```

Beispiel 2 – CodeBehind (gut): Verbinden des BitmapImage mit der Struktur, bevor dessen UriSource festgelegt wird.

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Beispiel 2: Code Behind (schlecht) – festlegen der UriSource des BitmapImage, bevor es mit der Struktur verbunden wird.

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>Optimierungen der Zwischenspeicherung

Optimierungen beim Zwischenspeichern gelten für Bilder, die [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) zum Laden von Inhalten aus einem App-Paket oder aus dem Internet verwenden. Der URI wird verwendet, um den zugrunde liegenden Inhalt eindeutig zu identifizieren. Intern sorgt er dafür, dass das XAML-Framework Inhalte nicht mehrmals herunterlädt oder decodiert. Stattdessen werden die zwischengespeicherten Software- oder Hardwareressourcen verwendet, um Inhalte mehrmals anzuzeigen.

Eine Ausnahme von dieser Optimierung besteht, wenn das Bild mehrmals in unterschiedlichen Auflösungen angezeigt wird (was explizit oder über automatische Decodierung in der richtigen Größe angegeben werden kann). Jeder Eintrag im Zwischenspeicher speichert auch die Auflösung des Bilds, und wenn XAML kein Bild mit einem Quell-URI finden kann, der der erforderlichen Auflösung entspricht, wird eine neue Version in dieser Größe decodiert. Die codierten Bilddaten werden jedoch nicht erneut heruntergeladen.

Aus diesem Grund sollten Sie [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) verwenden, wenn Sie Bilder aus einem App-Paket laden. Vermeiden Sie zudem die Verwendung eines Dateidatenstroms und von [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync), wenn diese nicht erforderlich sind.

### <a name="images-in-virtualized-panels-listview-for-instance"></a>Bilder in virtualisierten Panels (z. B. ListView)

Wenn ein Bild aus der Struktur entfernt wird (weil es von der App explizit entfernt wurde, oder weil es sich in einem modernen virtualisierten Panel befindet und implizit beim Bildlauf aus der Ansicht entfernt wurde), optimiert XAML die Speichernutzung, indem es die nicht mehr für das Bild benötigten Hardwareressourcen freigibt. Der Arbeitsspeicher wird nicht sofort freigegeben, sondern erst bei der Frameaktualisierung, die eine Sekunde nach Entfernung des Bildelements aus der Struktur stattfindet.

Folglich sollten Sie nach Möglichkeit moderne virtualisierte Panels zum Hosten von Listen mit Bildinhalten nutzen.

### <a name="software-rasterized-images"></a>Softwaregerasterte Bilder

Wenn ein Bild für einen nicht rechteckigen Pinsel oder ein [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid) verwendet wird, verwendet das Bild einen Softwarerasterungspfad, bei dem Bilder gar nicht skaliert werden. Zudem muss eine Kopie des Bilds sowohl im Arbeitsspeicher der Software als auch der Hardware gespeichert werden. Wenn z. B. ein Bild als Pinsel für eine Ellipse verwendet wird, wird das potenziell große Vollbild zweimal intern gespeichert. Bei Verwendung von **NineGrid** oder eines nicht rechteckigen Pinsels sollte die App die Bilder vorab auf die ungefähre Größe skalieren, in der sie gerendert werden.

### <a name="background-thread-image-loading"></a>Laden von Bildern in einem Hintergrundthread

XAML verfügt über eine interne Optimierung, mit der der Inhalt eines Bildes asynchron auf eine Oberfläche im Hardwarespeicher decodiert werden kann, ohne dass eine Zwischenoberfläche im Softwarespeicher benötigt wird. Dies reduziert Spitzen bei der Speicherauslastung und Renderinglatenz. Dieses Feature wird deaktiviert, wenn eine der folgenden Bedingungen erfüllt ist.

-   Das Bild wird als [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid) verwendet.
-   `CacheMode="BitmapCache"` wird auf das Bildelement oder ein beliebiges übergeordnetes Element festgelegt.
-   Der Bildpinsel ist nicht rechteckig (z. B. bei Anwendung auf eine Form oder auf Text).

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

Die [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Klasse tauscht interoperable, nicht komprimierte Bilder zwischen unterschiedlichen WinRT-Namespaces aus, z. B. [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder), Kamera-APIs und XAML. Diese Klasse vermeidet die zusätzliche Kopie, die in der Regel mit [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) erforderlich wäre. Dadurch reduzieren sich Spitzenlasten im Speicher sowie die Quelle-zu-Bildschirm-Latenz.

Die [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Klasse, die Quellinformationen bereitstellt, kann auch für ein benutzerdefiniertes [**IWICBitmap**](https://docs.microsoft.com/windows/desktop/api/wincodec/nn-wincodec-iwicbitmap)-Objekt konfiguriert werden. Dadurch wird ein erneut ladbarer Sicherungsspeicher bereitgestellt, mit dem die App Arbeitsspeicher nach Bedarf neu zuordnen kann. Dies ist ein erweiterter C++-Anwendungsfall.

Ihre App sollte [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) und [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) für die Interoperabilität mit anderen WinRT-APIs verwenden, die Bilder produzieren und nutzen. Zudem sollte die App beim Laden von nicht komprimierten Bilddaten anstelle von [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) die **SoftwareBitmapSource**-Klasse verwenden.

### <a name="use-getthumbnailasync-for-thumbnails"></a>Verwenden von „GetThumbnailAsync“ für Miniaturansichten

Ein Anwendungsfall für die Bildskalierung ist die Erstellung von Miniaturansichten. Sie könnten kleine Bildversionen zwar auch mit [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) bereitstellen, aber die Universelle Windows-Plattform (UWP) bietet noch effizientere APIs zum Abrufen von Miniaturansichten. [**Getthumbnailasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getthumbnailasync) stellt die Miniaturansichten für Images bereit, bei denen das Dateisystem bereits zwischengespeichert ist. Dadurch erhalten Sie eine noch bessere Leistung als bei den XAML-APIs, da das Bild weder geöffnet noch decodiert werden muss.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>Einmaliges Decodieren von Bildern

Damit Bilder nicht mehrmals decodiert werden, weisen Sie die [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)-Eigenschaft von einem URI aus zu, anstatt Speicherstreams zu verwenden. Das XAML-Framework kann den gleichen URI an mehreren Orten einem einzelnen decodierten Bild zuordnen. Für mehrere Speicherstreams mit den gleichen Daten ist dies allerdings nicht möglich. Hier erstellt das Framework für jeden Speicherstream ein eigenes decodiertes Bild.
