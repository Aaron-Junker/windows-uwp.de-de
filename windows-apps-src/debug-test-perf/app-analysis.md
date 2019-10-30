---
title: App-Analyse
description: Untersuchen Sie Leistungsprobleme der App
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e2977877b839f40e07b3eaa03b8349fb8439a401
ms.sourcegitcommit: 05be6929cd380a9dd241cc1298fd53f11c93d774
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062752"
---
# <a name="app-analysis-overview"></a>App-Analyse – Übersicht

Die App-Analyse ist ein Tool, mit dem Entwickler Vorab-Benachrichtigungen zu Leistungsproblemen erhalten. Bei der App-Analyse wird Ihr App-Code basierend auf Leistungsrichtlinien und bewährten Methoden ausgeführt.

Die App-Analyse identifiziert Probleme anhand eines Regelsatzes für häufige Leistungsprobleme, zu denen es für Apps kommen kann. Bei Bedarf wird bei der App-Analyse als Hilfe bei der Untersuchung auf das Zeitachsentool von Visual Studio, Quellinformationen und die Dokumentation verwiesen.

Die Regeln in der App-Analyse verweisen auf eine Richtlinie oder bewährte Methode, die für die Prüfung der App genutzt wird.

## <a name="decoded-image-size-larger-than-render-size"></a>Größe des decodierten Bilds übersteigt Rendergröße

Bilder werden in sehr hohen Auflösungen erfasst, was dazu führen kann, dass Apps mehr CPU beim Decodieren der Bilddaten und mehr Arbeitsspeicher nach dem Laden vom Datenträger belegen. Es ist jedoch nicht sehr sinnvoll, ein Bild in hoher Auflösung zu decodieren und im Arbeitsspeicher zu speichern, nur um es dann kleiner als in seiner ursprünglichen Größe anzuzeigen. Erstellen Sie stattdessen eine Version des Bilds in der genauen Größe, in der es auf dem Bildschirm gezeichnet wird. Verwenden Sie hierzu die Eigenschaften „DecodePixelWidth“ und „DecodePixelHeight“.

### <a name="impact"></a>Auswirkungen

Das Anzeigen von Bildern mit einer anderen als der nativen Größe kann sich negativ auf die CPU-Zeit (aufgrund der Decodierung auf die richtige Größe und der Downloadzeit) und den Arbeitsspeicher auswirken.

### <a name="causes-and-solutions"></a>Ursachen und Lösungen

#### <a name="image-is-not-being-set-asynchronously"></a>Bild wird nicht asynchron festgelegt

Für die App wird „SetSource()“ anstelle von „SetSourceAsync()“ verwendet. Vermeiden Sie beim Festlegen einer Datenstromquelle stets die Verwendung von [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource), und verwenden Sie stattdessen [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync), um Bilder asynchron zu decodieren. 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>Bild wird aufgerufen, wenn sich „ImageSource“ nicht in der Live-Struktur befindet

Für „BitmapImage“ wird die Verbindung mit der XAML-Live-Struktur hergestellt, nachdem der Inhalt mit „SetSourceAsync“ oder „UriSource“ festgelegt wurde. Fügen Sie stets ein [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Objekt an die Live-Struktur an, bevor Sie die Quelle festlegen. Dies erfolgt jedes Mal automatisch, wenn ein Bildelement oder ein Pinsel im Markupcode angegeben wird. Beispiele hierzu finden Sie weiter unten. 

**Livebaum-Beispiele**

Beispiel 1 (gut): URI (Uniform Resource Identifier) in Markup angegeben.

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Beispiel 2 – Markup: URI in CodeBehind angegeben.

```xml
<Image x:Name="myImage"/>
```

Beispiel 2 – CodeBehind (gut): Verbinden des BitmapImage mit der Struktur, bevor dessen UriSource festgelegt wird.

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Beispiel 2: Code Behind (schlecht) – festlegen der UriSource des BitmapImage, bevor es mit der Struktur verbunden wird.

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>Bildpinsel ist nicht rechteckig 

Wenn ein Bild für einen nicht rechteckigen Pinsel verwendet wird, wird für das Bild ein Softwarerasterungspfad genutzt, bei dem Bilder gar nicht skaliert werden. Zudem muss eine Kopie des Bilds sowohl im Arbeitsspeicher der Software als auch der Hardware gespeichert werden. Wenn z. B. ein Bild als Pinsel für eine Ellipse verwendet wird, wird das potenziell große Vollbild zweimal intern gespeichert. Bei Verwendung eines nicht rechteckigen Pinsels sollte Ihre App die Bilder vorab auf die ungefähre Größe skalieren, in der sie gerendert werden.

Alternativ dazu können Sie eine explizite Decodierungsgröße festlegen, um eine Version des Bilds mit der genauen Zeichnungsgröße auf dem Bildschirm zu erstellen. Verwenden Sie hierzu die Eigenschaften [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

Als Einheiten für [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) werden standardmäßig physische Pixel verwendet. Mit der [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype)-Eigenschaft kann dieses Verhalten geändert werden: Durch Festlegen von **DecodePixelType** auf **Logical** wird bei der Decodierungsgröße automatisch der aktuelle Skalierungsfaktor des Systems ähnlich wie andere XAML-Inhalte berücksichtigt. Daher ist es in der Regel sinnvoll, **DecodePixelType** auf **Logical** festzulegen, wenn beispielsweise **DecodePixelWidth** und **DecodePixelHeight** den Eigenschaften für die Höhe und Breite des Bildsteuerelements entsprechen sollen, in dem das Bild angezeigt wird. Beim Standardverhalten mit Verwendung von physischen Pixeln müssen Sie selbst den aktuellen Skalierungsfaktor des Systems berücksichtigen. Für den Fall, dass der Benutzer seine Anzeigeeinstellungen ändert, sollten Sie auf Benachrichtigungen über Skalierungsänderungen achten.

In einigen Fällen, in denen es nicht möglich ist, die passende Decodierungsgröße im Voraus zu bestimmen, sollten Sie die automatische Decodierung von XAML auf die richtige Größe nutzen. Dabei wird nach Möglichkeit versucht, das Bild in der passenden Größe zu decodieren, wenn keine explizite DecodePixelWidth/DecodePixelHeight angegeben ist.

Es wird empfohlen, eine explizite Decodierungsgröße festzulegen, wenn Sie die Größe des Bildinhalts bereits vorab kennen. Legen Sie gleichzeitig [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) auf **Logical** fest, wenn die angegebene Decodierungsgröße in Bezug auf andere XAML-Elementgrößen relativ ist. Wenn Sie z. B. die Inhaltsgröße explizit mit Image.Width und Image.Height festlegen, legen Sie DecodePixelType auf DecodePixelType.Logical fest, um die gleichen logischen Pixeldimensionen wie ein Bildsteuerelement zu verwenden. Verwenden Sie dann explizit BitmapImage.DecodePixelWidth und/oder BitmapImage.DecodePixelHeight, um die Größe des Bilds zu steuern und potenziell mehr Arbeitsspeicher einzusparen.

Beachten Sie, dass Image.Stretch beim Bestimmen der Größe des decodierten Inhalts berücksichtigt werden sollte.

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>Für in BitmapIcons verwendete Bilder erfolgt ein Fallback auf die Decodierung in die natürliche Größe 

Legen Sie eine explizite Decodierungsgröße fest, um eine Version des Bilds mit der genauen Zeichnungsgröße auf dem Bildschirm zu erstellen. Verwenden Sie hierzu die Eigenschaften [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>Für Bilder, die auf dem Bildschirm extrem groß erscheinen, erfolgt ein Fallback auf die Decodierung auf die natürliche Größe 

Für Bilder, die auf dem Bildschirm extrem groß erscheinen, wird ein Fallback auf die Decodierung auf die natürliche Größe durchgeführt. Legen Sie eine explizite Decodierungsgröße fest, um eine Version des Bilds mit der genauen Zeichnungsgröße auf dem Bildschirm zu erstellen. Verwenden Sie hierzu die Eigenschaften [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

#### <a name="image-is-hidden"></a>Bild ist ausgeblendet

Das Bild wird durch Festlegung von „Opacity“ auf 0 oder „Visibility“ auf „Collapsed“ für das Hostbildelement, den Pinsel oder ein beliebiges übergeordnetes Element ausgeblendet. Für Bilder, die auf dem Bildschirm nicht sichtbar sind, weil sie abgeschnitten wurden oder transparent sind, wird ggf. ein Fallback auf die Decodierung auf die natürliche Größe durchgeführt. 

#### <a name="image-is-using-ninegrid-property"></a>NineGrid-Eigenschaft wird für das Bild verwendet

Wenn ein Bild für ein [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid)-Element verwendet wird, wird für das Bild ein Softwarerasterungspfad genutzt, bei dem Bilder gar nicht skaliert werden. Zudem muss eine Kopie des Bilds sowohl im Arbeitsspeicher der Software als auch der Hardware gespeichert werden. Bei Verwendung von **NineGrid** sollte die App die Bilder vorab auf die ungefähre Größe skalieren, in der sie gerendert werden.

Für Bilder, die die NineGrid-Eigenschaft verwenden, wird ein Fallback auf die Decodierung auf die natürliche Größe durchgeführt. Sie können erwägen, dem Originalbild den NineGrid-Effekt hinzuzufügen.

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>DecodePixelWidth oder DecodePixelHeight ist auf einen höheren Wert als für die Anzeige des Bilds auf dem Bildschirm festgelegt 

Wenn „DecodePixelWidth/Height“ explizit größer als die Bildanzeige auf dem Bildschirm festgelegt sind, belegt die App unnötigen zusätzlichen Arbeitsspeicher (bis zu 4 Byte pro Pixel), was bei großen Bildern deutlich ins Gewicht fällt. Zudem wird das Bild mit bilinearer Skalierung verkleinert und kann bei großen Skalierungsfaktoren unscharf dargestellt werden.

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>Bild wird beim Produzieren eines Drag&Drop-Bilds decodiert

Legen Sie eine explizite Decodierungsgröße fest, um eine Version des Bilds mit der genauen Zeichnungsgröße auf dem Bildschirm zu erstellen. Verwenden Sie hierzu die Eigenschaften [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) und [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

## <a name="collapsed-elements-at-load-time"></a>Reduzierte Elemente zur Ladezeit

Eine häufiges Muster bei Apps ist, UI-Elemente anfänglich auszublenden und erst später anzuzeigen. In den meisten Fällen sollten diese Elemente mit „x:Load“ oder „x:DeferLoadStrategy“ zurückgestellt werden, um den Aufwand für die Erstellung des Elements zur Ladezeit zu vermeiden.

Hierzu gehören Fälle, in denen ein Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand genutzt wird, um Elemente bis zu einem späteren Zeitpunkt auszublenden.

### <a name="impact"></a>Auswirkungen

Reduzierte Elemente werden zusammen mit anderen Elementen geladen und tragen zu einer Verlängerung der Ladezeit bei.

### <a name="cause"></a>Ursache

Diese Regel wurde ausgelöst, weil ein Element zur Ladezeit reduziert wurde. Das Reduzieren eines Elements oder das Festlegen der Deckkraft auf 0 verhindert nicht, dass das Element erstellt wird. Diese Regel kann durch eine App verursacht werden, für die ein Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand verwendet wird, dessen Standardeinstellung „false“ lautet.

### <a name="solution"></a>Lösung

Mit [x:Load](../xaml-platform/x-load-attribute.md) oder [x:DeferLoadStrategy](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute) können Sie das Laden eines UI-Elements verschieben und es erst dann laden, wenn es benötigt wird. Dies ist eine gute Möglichkeit, die Verarbeitung der UI zu verzögern, die im ersten Frame nicht sichtbar ist. Sie können angeben, ob das Element bei Bedarf oder im Rahmen eines Satzes mit verzögerter Logik geladen werden soll. Rufen Sie zum Auslösen des Ladens „findName“ für das Element auf, das geladen werden soll. „x:Load“ erweitert die Funktionen von „x:DeferLoadStrategy“ und ermöglicht, dass Elemente entladen und beim Laden über „x:Bind“ gesteuert werden.

In einigen Fällen ist die Verwendung von „findName“ zum Anzeigen eines UI-Elements eventuell nicht die Lösung. Dies gilt, wenn Sie Folgendes erwarten: Ein erheblicher Teil der UI soll nach dem Klicken auf eine Schaltfläche und mit sehr geringer Latenz realisiert werden. In diesem Fall sollten Sie eine geringere UI-Latenz auf Kosten des zusätzlichen Speichers in Kauf nehmen und „x: DeferLoadStrategy“ verwenden sowie die „Sichtbarkeit“ des Elements, das Sie realisieren möchten, auf „Reduziert“ festlegen. Nachdem die Seite geladen wurde und der UI-Thread frei ist, können Sie „findName“ je nach Bedarf zum Laden der Elemente aufrufen. Die Elemente sind für den Benutzer erst sichtbar, nachdem Sie die Sichtbarkeit des Elements auf „Visible“ (Sichtbar) festgelegt haben.

## <a name="listview-is-not-virtualized"></a>ListView ist nicht virtualisiert

Die UI-Virtualisierung ist die wichtigste Möglichkeit zur Leistungsoptimierung für Sammlungen. Dies bedeutet, dass die Benutzeroberflächenelemente, die die Objekte darstellen, bei Bedarf erstellt werden. Für ein Elementsteuerelement, das an eine Sammlung mit 1.000 Elementen gebunden ist, wäre es eine Verschwendung von Ressourcen, die UI für alle Elemente gleichzeitig zu erstellen. Sie können sowieso nicht alle gleichzeitig angezeigt werden. ListView und GridView (und andere von ItemsControl abgeleitete Standardsteuerelemente) führen die Virtualisierung der Benutzeroberfläche für Sie durch. Wenn Elemente kurz davor sind, per Bildlauf in der Ansicht angezeigt zu werden (einige Seiten davon entfernt), generiert das Framework die Benutzeroberfläche für die Elemente und speichert sie zwischen. Wenn es unwahrscheinlich ist, dass die Elemente erneut angezeigt werden, gibt das Framework den Arbeitsspeicher wieder frei.

Die UI-Virtualisierung ist nur einer von mehreren Schlüsselfaktoren zur Verbesserung der Leistung von Sammlungen. Die Reduzierung der Komplexität von Sammlungselementen und die Datenvirtualisierung sind zwei weitere wichtige Aspekte zur Verbesserung der Sammlungsleistung. Weitere Informationen zur Verbesserung der Sammlungsleistung in ListViews und GridViews finden Sie in den Artikeln [Optimieren der ListView- und GridView-Benutzeroberfläche](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) und [Virtualisierung von „ListView“- und „GridView“-Daten](https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Auswirkungen

Ein nicht virtualisiertes ItemsControl-Element führt zu einer Verlängerung der Ladezeit und einer erhöhten Ressourcennutzung, da mehr untergeordnete Elemente als erforderlich geladen werden.

### <a name="cause"></a>Ursache

Das Viewport-Konzept ist für die UI-Virtualisierung entscheidend, da das Framework die Elemente erstellen muss, die voraussichtlich angezeigt werden. Allgemein gilt: Der Viewport einer ItemsControl-Klasse entspricht dem Umfang des logischen Steuerelements. So entspricht beispielsweise der Viewport einer ListView-Klasse der Breite und Höhe des ListView-Elements. Einige Panels gewähren untergeordneten Elementen unbegrenzten Speicherplatz. Beispiele hierfür sind ScrollViewer und ein Grid-Objekt mit Zeilen oder Spalten mit automatischer Größenanpassung. Wird ein virtualisiertes ItemsControl-Objekt in einem dieser Panels platziert, nimmt es ausreichend Platz ein, um alle Elemente anzuzeigen, sodass die Virtualisierung „vereitelt“ wird. 

### <a name="solution"></a>Lösung

Stellen Sie die Virtualisierung wieder her, indem Sie die Breite und Höhe für das verwendete ItemsControl-Element festlegen.

## <a name="ui-thread-blocked-or-idle-during-load"></a>UI-Thread ist während des Ladens blockiert oder im Leerlauf

Die Blockierung des UI-Threads bezieht sich auf synchrone Aufrufe von Funktionen, die außerhalb des Threads ausgeführt werden und den UI-Thread blockieren.  

Eine vollständige Liste mit bewährten Methoden zur Verbesserung der Startleistung Ihrer App finden Sie unter [Bewährte Methoden für die Leistung Ihrer App beim Starten](https://docs.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance) und [Aufrechterhalten der Reaktionsfähigkeit des UI-Threads](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive).

### <a name="impact"></a>Auswirkungen

Ein blockierter oder im Leerlauf befindlicher UI-Thread während der Ladezeit verhindert den Layoutvorgang und andere UI-Vorgänge und führt somit zu einer längeren Startdauer.

### <a name="cause"></a>Ursache

Plattformcode für die Benutzeroberfläche und Ihr App-Code für die Benutzeroberfläche werden im gleichen UI-Thread ausgeführt. Für diesen Thread kann nur jeweils eine Anweisung ausgeführt werden. Wenn Ihr App-Code also zu lange für das Verarbeiten eines Ereignisses benötigt, kann das Framework das Layout nicht ausführen bzw. keine neuen Ereignisse auslösen, die Interaktionen des Benutzers darstellen. Dies bedeutet, dass die Reaktionsfähigkeit Ihrer App direkt davon abhängt, ob der UI-Thread zum Durchführen von Arbeit verfügbar ist.

### <a name="solution"></a>Lösung

Ihre App kann interaktiv sein, selbst wenn Teile der App noch nicht voll funktionsfähig sind. Wenn von Ihrer App beispielsweise Daten angezeigt werden, deren Abruf einige Zeit in Anspruch nimmt, können Sie diesen Code separat vom Startcode der App ausführen, indem Sie die Daten asynchron abrufen. Wenn die Daten verfügbar sind, füllen Sie die Benutzeroberfläche der App mit den Daten auf. Zur Verbesserung der Reaktionsfähigkeit der App stellt die Plattform asynchrone Versionen vieler APIs bereit. Eine asynchrone API stellt sicher, dass Ihr aktiver Ausführungsthread nie für längere Zeit blockiert wird. Wenn Sie ein API-Element aus dem UI-Thread heraus aufrufen, verwenden Sie die asynchrone Version, sofern diese verfügbar ist.

## <a name="binding-is-being-used-instead-of-xbind"></a>{Binding} wird anstelle von {x:Bind} verwendet

Diese Regel wird ausgelöst, wenn in der App eine {Binding}-Anweisung verwendet wird. Zur Verbesserung der App-Leistung sollte für Apps die Nutzung von {x:Bind} erwogen werden.

### <a name="impact"></a>Auswirkungen

Für {Binding} ist mehr Zeit und Arbeitsspeicher als für {x:Bind} erforderlich.

### <a name="cause"></a>Ursache

Für die App wird {Binding} anstelle von {x:Bind} verwendet. {Binding} ist mit einer hohen Arbeitssatz- und CPU-Auslastung verbunden. Die Erstellung einer Bindung führt zu einer Reihe von Zuordnungen, und die Aktualisierung eines Bindungsziels kann zu Reflektion und Boxing führen.

### <a name="solution"></a>Lösung

Verwenden Sie die {x:Bind}-Markuperweiterung, mit der Bindungen zur Buildzeit kompiliert werden. {x:Bind}-Bindungen (häufig als kompilierte Bindungen bezeichnet) bieten eine hervorragende Leistung, stellen die Validierung Ihrer Bindungsausdrücke bei der Kompilierung bereit und unterstützen das Debuggen, indem Sie die Möglichkeit erhalten, Haltepunkte in den Codedateien festzulegen, die als Teilklasse für die Seite generiert werden. 

Beachten Sie, dass x:Bind nicht in allen Fällen geeignet ist, z. B. spät gebundenen Szenarien. Eine vollständige Liste mit Fällen, die mit {x:Bind} nicht abgedeckt werden, finden Sie in der {x:Bind}-Dokumentation.

## <a name="xname-is-being-used-instead-of-xkey"></a>x:Name wird anstelle von x:Key verwendet

Ressourcenwörterbücher werden normalerweise zum Speichern Ihrer Ressourcen auf globalerer Ebene verwendet. Dies betrifft Ressourcen, auf die von der App an mehreren Stellen verwiesen werden soll, z. B. Stile, Pinsel, Vorlagen usw. Ressourcenwörterbücher wurden grundsätzlich dahingehend optimiert, dass nur angeforderte Ressourcen instanziiert werden. Es gibt aber einige Stellen, an denen Vorsicht geboten ist.

### <a name="impact"></a>Auswirkungen

Jede Ressource mit „x:Name“ wird sofort nach dem Erstellen des Ressourcenwörterbuchs instanziiert. Der Grund: „x:Name“ teilt der Plattform mit, dass Ihre App Feldzugriff auf diese Ressource benötigt. Daher muss die Plattform etwas erstellen, für das ein Verweis eingerichtet werden kann.

### <a name="cause"></a>Ursache

Die App legt „x:Name“ für eine Ressource fest.

### <a name="solution"></a>Lösung

Verwenden Sie „x:Key“ anstelle von „x:Name“, wenn Sie nicht aus CodeBehind-Code auf Ressourcen verweisen.

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>Sammlungssteuerelement verwendet ein Panel ohne Virtualisierung

Wenn Sie eine benutzerdefinierte ItemsPanel-Vorlage bereitstellen (siehe ItemsPanel), ist es wichtig, dass Sie ein Virtualisierungspanel wie ItemsWrapGrid oder ItemsStackPanel verwenden. Wenn Sie VariableSizedWrapGrid, WrapGrid oder StackPanel verwenden, erhalten Sie keine Virtualisierung. Darüber hinaus werden die folgenden ListView-Ereignisse nur ausgelöst, wenn ItemsWrapGrid oder ItemsStackPanel verwendet werden: ChoosingGroupHeaderContainer, ChoosingItemContainer und ContainerContentChanging.

Die UI-Virtualisierung ist die wichtigste Möglichkeit zur Leistungsoptimierung für Sammlungen. Dies bedeutet, dass die Benutzeroberflächenelemente, die die Objekte darstellen, bei Bedarf erstellt werden. Für ein Elementsteuerelement, das an eine Sammlung mit 1.000 Elementen gebunden ist, wäre es eine Verschwendung von Ressourcen, die UI für alle Elemente gleichzeitig zu erstellen. Sie können sowieso nicht alle gleichzeitig angezeigt werden. ListView und GridView (und andere von ItemsControl abgeleitete Standardsteuerelemente) führen die Virtualisierung der Benutzeroberfläche für Sie durch. Wenn Elemente kurz davor sind, per Bildlauf in der Ansicht angezeigt zu werden (einige Seiten davon entfernt), generiert das Framework die Benutzeroberfläche für die Elemente und speichert sie zwischen. Wenn es unwahrscheinlich ist, dass die Elemente erneut angezeigt werden, gibt das Framework den Arbeitsspeicher wieder frei.

Die UI-Virtualisierung ist nur einer von mehreren Schlüsselfaktoren zur Verbesserung der Leistung von Sammlungen. Die Reduzierung der Komplexität von Sammlungselementen und die Datenvirtualisierung sind zwei weitere wichtige Aspekte zur Verbesserung der Sammlungsleistung. Weitere Informationen zur Verbesserung der Sammlungsleistung in ListViews und GridViews finden Sie in den Artikeln [Optimieren der ListView- und GridView-Benutzeroberfläche](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) und [Virtualisierung von „ListView“- und „GridView“-Daten](https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Auswirkungen

Ein nicht virtualisiertes ItemsControl-Element führt zu einer Verlängerung der Ladezeit und einer erhöhten Ressourcennutzung, da mehr untergeordnete Elemente als erforderlich geladen werden.

### <a name="cause"></a>Ursache

Sie verwenden ein Panel, von dem die Virtualisierung nicht unterstützt wird.

### <a name="solution"></a>Lösung

Verwenden Sie ein Virtualisierungspanel wie ItemsWrapGrid oder ItemsStackPanel.

## <a name="accessibility-uia-elements-with-no-name"></a>Eingabehilfen: UIA-Elemente ohne Namen

Im XAML-Code können Sie einen Namen angeben, indem Sie „AutomationProperties.Name“ festlegen. Bei vielen Automatisierungspeers wird ein Standardname für UIA angegeben, wenn „AutomationProperties.Name“ nicht festgelegt wurde. 

### <a name="impact"></a>Auswirkungen

Wenn ein Benutzer auf ein Element ohne Namen trifft, ist häufig nicht erkennbar, worauf sich das Element bezieht. 

### <a name="cause"></a>Ursache

Der UIA-Name des Elements ist NULL oder leer. Mit dieser Regel wird nicht der Wert von „AutomationProperties.Name“ überprüft, sondern was UIA „sieht“.

### <a name="solution"></a>Lösung

Legen Sie die AutomationProperties.Name-Eigenschaft im XAML-Code des Steuerelements auf eine geeignete lokalisierte Zeichenfolge fest.

In einigen Fällen ist der richtige Anwendungspatch nicht das Angeben eines Namens, sondern das Entfernen des UIA-Elements aus allen Strukturen mit Ausnahme der Raw-Strukturen. Dies können Sie in XAML tun, indem Sie `AutomationProperties.AccessibilityView = "Raw"`festlegen.

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>Eingabehilfen: UIA-Elemente mit dem gleichen ControlType sollten nicht den gleichen Namen haben.

Zwei UIA-Elemente mit demselben übergeordneten UIA-Element dürfen nicht den gleichen Namen und ControlType haben. Zwei Steuerelemente können den gleichen Namen haben, wenn sie über unterschiedliche ControlTypes verfügen. 

Mit dieser Regel wird keine Prüfung auf doppelte Namen mit unterschiedlichen übergeordneten Elementen durchgeführt. In den meisten Fällen ist es aber ratsam, innerhalb eines Gesamtfensters auch dann keine doppelten Namen und ControlTypes zu verwenden, wenn die übergeordneten Elemente unterschiedlich sind. Ein Fall, in dem doppelte Namen innerhalb eines Fensters akzeptabel sind, ist die Nutzung von zwei Listen mit identischen Elementen. Hierbei wird erwartet, dass die Listenelemente identische Namen und ControlTypes haben.

### <a name="impact"></a>Auswirkungen

Wenn ein Benutzer auf ein Element mit dem gleichen Namen und ControlType wie ein anderes Element mit demselben übergeordneten UIA-Element trifft, kann er zwischen diesen Elementen keinen Unterschied feststellen.

### <a name="cause"></a>Ursache

UIA-Elemente mit demselben übergeordneten UIA-Element haben den gleichen Namen und ControlType.

### <a name="solution"></a>Lösung

Legen Sie im XAML-Code mit AutomationProperties.Name einen Namen fest. Verwenden Sie in Listen, in denen dies häufiger vorkommt, eine Bindung, um den Wert von AutomationProperties.Name an eine Datenquelle zu binden.


