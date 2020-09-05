---
title: Erstellen von Datenbindungen
description: Befolgen Sie dieses Tutorial zum Erstellen von Datenbindungen mithilfe von XAML und C#, um einen direkten Link zwischen Ihrer Benutzeroberfläche und den Daten bilden.
keywords: XAML, UWP, Erste Schritte
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5d3363dcc47ef43fe65b3c954b213a81cc5165e1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166284"
---
# <a name="tutorial-create-data-bindings"></a>Tutorial: Erstellen von Datenbindungen

Angenommen, du hast eine schöne Benutzeroberfläche entworfen und implementiert, die mit Platzhalterbildern, „lorem ipsum“-Standardtext und Steuerelementen gefüllt ist, die noch keine Funktion haben. Als Nächstes möchtest du sie von einem Prototypentwurf in eine echten App umwandeln und mit echten Daten verbinden.

In diesem Tutorial erfährst du, wie du deine Textbausteine durch Datenbindungen ersetzt und andere direkte Links zwischen der Benutzeroberfläche und den Daten erstellst. Außerdem erfährst du, wie du deine Daten für die Anzeige formatierst oder konvertierst und die Benutzeroberfläche und die Daten synchron hältst. Wenn du dieses Tutorial durcharbeitest, kannst du den XAML- und C#-Code vereinfachen und besser organisieren, sodass er einfacher zu verwalten und zu erweitern ist.

Du beginnst mit einer vereinfachten Version des PhotoLab-Beispiels. Dieses Starterversion umfasst die vollständige Datenebene und ein grundlegendes XAML-Layout und lässt viele kleinere Features aus, damit der Code einfacher zu durchsuchen ist. In diesem Tutorial lernen Sie nicht das Erstellen der vollständigen App. Lesen Sie daher die endgültige Version zu Features wie benutzerdefinierten Animationen und adaptive Layouts. Die endgültige Version findest du im Stammordner des Repositorys [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).

Die Beispiel-App für PhotoLab verfügt über zwei Seiten. Die _Hauptseite_ zeigt eine Ansicht der Foto-Galerie zusammen mit einigen Informationen über jede Bilddatei an.

![MainPage](../design/basics/images/xaml-basics/mainpage.png)

Die *Detailseite* zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Voraussetzungen

+ Visual Studio 2019: [Visual Studio 2019 herunterladen](https://visualstudio.microsoft.com/downloads/) (Community-Edition ist kostenlos)
+ Windows 10 SDK (10.0.17763.0 oder höher):  [Das aktuelle Windows SDK herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, Version 1809 oder höher

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Startcode von GitHub abrufen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels.

1. Zur GitHub-Seite für das Beispiel navigieren: [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
2. Als Nächstes müssen Sie das Beispiel klonen oder herunterladen. Wähle die Schaltfläche **Clone or download** (Klonen oder Herunterladen) aus. Ein Untermenü erscheint.
    ![Das Menü „Clone or download“ (Klonen oder Herunterladen) auf der GitHub-Seite des PhotoLab-Beispiels](../design/basics/images/xaml-basics/clone-repo.png)

    **Wenn Sie mit GitHub nicht vertraut sind:**

    ein. Wähle **Download ZIP** aus, und speichere die Datei lokal. Es wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.

    b. Entpacken Sie die Datei. Verwenden Sie den Datei-Explorer, um zu der soeben heruntergeladenen ZIP-Datei zu navigieren, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Alle extrahieren** aus.

    c. Navigiere zu deiner lokalen Kopie des Beispiels, und wechsle zum Verzeichnis `Windows-appsample-photo-lab-master\xaml-basics-starting-points\data-binding`.

    **Wenn Sie mit GitHub vertraut sind:**

    ein. Klonen Sie den Master-Branch des Repositorys lokal.

    b. Wechsle zum `Windows-appsample-photo-lab\xaml-basics-starting-points\data-binding`-Verzeichnis.

3. Doppelklicken Sie auf `Photolab.sln`, um die Projektmappe in Visual Studio zu öffnen.

## <a name="part-1-replace-the-placeholders"></a>Teil 1: Ersetzen der Platzhalter

Hier erstellen Sie einmalige Bindungen der XAML-Datenvorlage, um echte Bilder und Bildmetadaten von Bildern anstelle von Platzhalterinhalten anzuzeigen.

Einmalige Bindungen eignen sich für schreibgeschützte, unveränderliche Daten, d. h., sie sind sehr leistungsfähig und einfach zu erstellen und ermöglichen die Anzeige großer Datensätze in den Steuerelementen `GridView` und `ListView`.

### <a name="replace-the-placeholders-with-one-time-bindings"></a>Ersetzen der Platzhalter durch einmalige Bindungen

1. Öffnen Sie den Ordner `xaml-basics-starting-points\data-binding`, und starten Sie die Datei `PhotoLab.sln` in Visual Studio.

2. Stellen Sie sicher, dass Ihre **Projektmappenplattform** auf x86 oder x64 und nicht auf ARM festgelegt ist, und führen Sie die App aus. Hierbei wird der Status der App mit UI-Platzhaltern angezeigt, bevor Bindungen hinzugefügt wurden.

    ![Ausführen der App mit Platzhalterbildern und -text](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. Öffne „MainPage.xaml“, und suche nach einer `DataTemplate` mit dem Namen **ImageGridView_DefaultItemTemplate**. Aktualisiere diese Vorlage, sodass Datenbindungen verwendet werden.

    **Vorher:**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    Der Wert `x:Key` wird von `ImageGridView` verwendet, um diese Vorlage zum Anzeigen von Datenobjekten auszuwählen.

4. Füge der Vorlage einen Wert `x:DataType` hinzu.

    **Nachher:**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
    ```

    `x:DataType` gibt an, für welchen Typ diese Vorlage ausgelegt ist. In diesem Fall handelt es sich um eine Vorlage für die `ImageFileInfo`-Klasse (wobei `local:` den lokalen Namespace gemäß der Definition in einer xmlns-Deklaration im oberen Bereich der Datei angibt).

    `x:DataType` wird benötigt, wenn `x:Bind`-Ausdrücke in einer Datenvorlage verwendet werden, wie nachfolgend beschrieben.

5. Suche in `DataTemplate` das Element `Image` mit dem Namen `ItemImage`, und ersetze den Wert `Source` wie gezeigt.

    **Vorher:**

    ```xaml
    <Image x:Name="ItemImage"
           Source="/Assets/StoreLogo.png"
           Stretch="Uniform" />
    ```

    **Nachher:**

    ```xaml
    <Image x:Name="ItemImage"
           Source="{x:Bind ImageSource}"
           Stretch="Uniform" />
    ```

    `x:Name` identifiziert ein XAML-Element, damit du an anderer Stelle im XAML-Code und im CodeBehind darauf verweisen kannst.

    `x:Bind`-Ausdrücke statten die UI-Eigenschaft mit einem Wert aus, indem sie den Wert von einer **data-object**-Eigenschaft abrufen. In Vorlagen ist die angegebene Eigenschaft eine Eigenschaft gemäß der Festlegung in `x:DataType`. In diesem Fall ist die Datenquelle somit die `ImageFileInfo.ImageSource`-Eigenschaft.

    > [!NOTE]
    > Der Wert `x:Bind` informiert ebenfalls den Editor über den Datentyp, damit du IntelliSense verwenden kannst, anstatt den Namen der Eigenschaft in den `x:Bind`-Ausdruck einzugeben. Probiere dies mit dem soeben eingefügten Code aus: Platziere den Cursor unmittelbar nach `x:Bind`, und drücke die LEERTASTE, um eine Liste der Eigenschaften anzuzeigen, für die du eine Bindung erstellen kannst.

6. Ersetze die Werte der anderen UI-Steuerelemente auf die gleiche Weise. (Probiere es mithilfe von IntelliSense anstatt Kopieren/Einfügen aus.)

    **Vorher:**

    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <muxc:RatingControl Value="3" ... />
    ```

    **Nachher:**

    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <muxc:RatingControl Value="{x:Bind ImageRating}" ... />
    ```

Führe die App aus, um herauszufinden, wie sie bisher aussieht. Keine Platzhalter mehr. Das ist ein guter Ausgangspunkt.

![Ausführen der App mit echten Bildern und Texten anstelle von Platzhaltern](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> Wenn du weiter experimentieren möchtest, füge der Datenvorlage einen neuen TextBlock hinzu, und verwende dabei den IntelliSense-Trick mit „x:Bind“, um eine Eigenschaft zum Anzeigen zu suchen.

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>Teil 2: Verwenden der Bindung zum Verbinden der Galeriebenutzeroberfläche mit den Bildern

Hier erstellst du einmalige Bindungen in XAML-Seiten für die Verbindung der Galerieansicht mit der Bildersammlung und ersetzt dabei den vorhandenen prozeduralen Code, der dies im CodeBehind durchführt. Außerdem erstellst du eine Schaltfläche **Löschen**, um zu sehen, wie sich die Galerieansicht ändert, wenn Bilder aus der Sammlung entfernt werden. Gleichzeitig erfährst du, wie Ereignisse an Ereignishandler gebunden werden, um eine größere Flexibilität als bei herkömmlichen Ereignishandlern zu erreichen.

Alle bisher behandelten Bindungen befinden sich innerhalb von Datenvorlagen und beziehen sich auf Eigenschaften der Klasse, die durch den Wert `x:DataType` angegeben wird. Wozu dient der restliche XAML-Code auf der Seite?

`x:Bind`-Ausdrücke außerhalb von Datenvorlagen sind immer an die Seite selbst gebunden. Das bedeutet, dass du auf beliebige Elemente im CodeBehind oder in XAML-Deklarationen verweisen kannst, einschließlich benutzerdefinierter Eigenschaften und der Eigenschaften anderer Steuerelemente der Benutzeroberfläche auf der Seite (sofern sie einen Wert für `x:Name` aufweisen).

Im PhotoLab-Beispiel kann eine solche Bindung verwendet werden, um das `GridView`-Hauptsteuerelement direkt mit der Sammlung von Bildern zu verbinden, anstatt dies im CodeBehind zu erledigen. Später folgen weitere Beispiele.

### <a name="bind-the-main-gridview-control-to-the-images-collection"></a>Binden des GridView-Hauptsteuerelements an die Bildersammlung

1. Suche in „MainPage.xaml.cs“ die `GetItemsAsync`-Methode, und entferne den Code, der `ItemsSource` festlegt.

    **Vorher:**

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    **Nachher:**

    ```csharp
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. Suche in „MainPage.xaml“ das `GridView` mit dem Namen `ImageGridView`, und füge ein `ItemsSource`-Attribut hinzu. Verwende für den Wert einen `x:Bind`-Ausdruck, der sich auf die im CodeBehind implementierte `Images`-Eigenschaft bezieht.

    **Vorher:**

    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Nachher:**

    ```xaml
    <GridView x:Name="ImageGridView"
              ItemsSource="{x:Bind Images}"
    ```

    Die `Images`-Eigenschaft ist vom Typ `ObservableCollection<ImageFileInfo>`, sodass die einzelnen Elemente, die in `GridView` angezeigt werden, vom Typ `ImageFileInfo` sind. Dies entspricht dem Wert `x:DataType`, der in Teil 1 beschrieben wurde.

Alle bisher untersuchten Bindungen sind einmalige, schreibgeschützte Bindungen. Dies ist das Standardverhalten für einfache `x:Bind`-Ausdrücke. Die Daten werden nur bei der Initialisierung hochgeladen, sodass die Bindungen eine hohe Leistung aufweisen – ideal für die Unterstützung mehrerer komplexer Ansichten großer Datasets.

Selbst die soeben hinzugefügte `ItemsSource`-Bindung ist eine einmalige, schreibgeschützte Bindung für einen unveränderlichen Wert der Eigenschaft, hier gibt es jedoch einen wichtigen Unterschied. Der unveränderliche Wert der `Images`-Eigenschaft ist eine einzelne, spezifische Instanz einer Sammlung, die wie hier gezeigt einmal initialisiert wird.

```csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

Der Wert der `Images`-Eigenschaft ändert sich nie, aber da die Eigenschaft den Typ `ObservableCollection<T>` aufweist, können die *Inhalte* der Sammlung geändert werden. Die Bindung erkennt dann automatisch die Änderungen und aktualisiert die Benutzeroberfläche.

Um dies zu testen, fügen wir vorübergehend eine Schaltfläche hinzu, die das derzeit ausgewählte Bild löscht. In der endgültigen Version ist diese Schaltfläche nicht vorhanden, da die Auswahl eines Bildes zu einer Detailseite führt. Das Verhalten von `ObservableCollection<T>` ist jedoch im letzten PhotoLab-Beispiel dennoch wichtig, da der XAML-Code im Seitenkonstruktor initialisiert wird (über den `InitializeComponent`-Methodenaufruf). Die Sammlung `Images` wird später in der `GetItemsAsync`-Methode mit Daten aufgefüllt.

### <a name="add-a-delete-button"></a>Hinzufügen einer Schaltfläche zum Löschen

1. Suche in „MainPage.xaml“ die `CommandBar` mit dem Namen **MainCommandBar**, und füge vor der Schaltfläche zum Zoomen eine neue Schaltfläche hinzu. (Die Zoomsteuerelemente funktionieren noch nicht. Sie werden im nächsten Teil des Tutorials eingebunden.)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    Wenn du bereits mit XAML vertraut bist, macht dieser Wert `Click` möglicherweise einen ungewöhnlichen Eindruck. In früheren Versionen von XAML musstest du diesen Wert auf eine Methode mit einer bestimmten Ereignishandlersignatur festlegen, normalerweise einschließlich Parametern für den Absender des Ereignisses und eines ereignisspezifischen Argumentobjekts. Sie können diese Technik auch weiterhin verwenden, wenn Sie die Ereignisargumente benötigen, aber mit `x:Bind` ist auch eine Verbindung mit anderen Methoden möglich. Wenn du beispielsweise die Ereignisdaten nicht benötigst, kannst du eine Verbindung mit Methoden herstellen, die keine Parameter aufweisen, wie hier gezeigt.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. Füge „MainPage.xaml.cs“ die `DeleteSelectedImage`-Methode hinzu.

    ```csharp
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    Diese Methode löscht das ausgewählte Bild aus der Sammlung `Images`.

Führe jetzt die App aus, und verwende die Schaltfläche, um einige Bilder zu löschen. Wie Sie sehen, wird die Benutzeroberfläche dank der Datenbindung und dem Typ `ObservableCollection<T>` automatisch aktualisiert.

> [!Note]
> Mit diesem Code wird nur die `ImageFileInfo`-Instanz aus der `Images`-Sammlung in der ausgeführten App gelöscht. Die Bilddatei wird nicht vom Computer gelöscht.

## <a name="part-3-set-up-the-zoom-slider"></a>Teil 3: Einrichten des Schiebereglers zum Zoomen

In dieser Phase erstellen wir unidirektionale Bindungen von einem Steuerelement in der Datenvorlage zum Schieberegler zum Zoomen, der sich außerhalb der Vorlage befindet. Außerdem erfährst du, dass du die Datenbindung mit vielen Eigenschaften des Steuerelements verwenden kannst, nicht nur den offensichtlichen wie `TextBlock.Text` und `Image.Source`.

### <a name="bind-the-image-data-template-to-the-zoom-slider"></a>Binden der Bilddatenvorlage an den Schieberegler zum Zoomen

+ Suche die `DataTemplate` mit dem Namen `ImageGridView_DefaultItemTemplate`, und ersetze die Werte `**Height**` und `Width` des Steuerelements `Grid` im oberen Bereich der Vorlage.

    **Vorher**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```

    **Nachher**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Ist dir aufgefallen, dass es sich um `Binding`-Ausdrücke und nicht um `x:Bind`-Ausdrücke handelt? Dies ist die alte Vorgehensweise für Datenbindungen, die überwiegend veraltet ist. `x:Bind` weist fast alle Funktionen von `Binding` und viele darüber hinausgehenden auf. Wenn du jedoch `x:Bind` in einer Datenvorlage verwendest, erfolgt die Bindung an den im Wert `x:DataType` deklarierten Typ. Wie bindest du also ein Element der Vorlage an eines in der XAML-Seite oder im CodeBehind? Du musst einen `Binding`-Ausdruck im alten Stil verwenden.

In `Binding`-Ausdrücken wird der Wert `x:DataType` nicht erkannt, aber diese `Binding`-Ausdrücke weisen `ElementName`-Werte auf, die nahezu gleich funktionieren. Diese teilen dem Bindungsmodul mit, dass **Binding Value** eine Bindung für die `Value`-Eigenschaft des angegebenen Elements auf der Seite ist (d. h. das Element mit dem Wert `x:Name`). Wenn du eine Eigenschaft im CodeBehind binden möchtest, würde dies ```{Binding MyCodeBehindProperty, ElementName=page}``` ähneln, wobei `page` dem Wert `x:Name` entspricht, der im Element `Page` im XAML-Code festgelegt ist.

> [!NOTE]
> In der Standardeinstellung sind `Binding`-Ausdrücke *unilateral*, d. h., sie aktualisieren die Benutzeroberfläche automatisch, wenn die Eigenschaft des gebundenen Werts geändert wird.
>
> Im Gegensatz dazu ist für `x:Bind` die Standardeinstellung *einmalig*, d. h., jegliche Änderungen an der gebundenen Eigenschaft werden ignoriert. Dies ist die Standardeinstellung, da es die leistungsstärkste Option ist und die meisten Bindungen zu statischen, schreibgeschützten Daten erstellt werden.
>
> Wenn du `x:Bind` mit Eigenschaften verwendet, deren Werte geändert werden können, solltest du daher unbedingt `Mode=OneWay`oder `Mode=TwoWay` hinzufügen. Im nächsten Abschnitt siehst du ein Beispiel dafür.

Führe die App aus, und verwende den Schieberegler, um die Größe der Bildvorlage zu ändern. Wie du sehen kannst, ist dies auch ohne viel Code bereits eindrucksvoll.

![Ausführen der App mit angezeigtem Schieberegler zum Zoomen](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> Versuche als Herausforderung, andere Eigenschaften der Benutzeroberfläche an die `Value`-Eigenschaft des Schiebereglers zum Zoomen oder eines anderen Schiebereglers zu binden, den du anschließend hinzufügst. Du kannst z. B. die `FontSize`-Eigenschaft von `TitleTextBlock` an einen neuen Schieberegler mit dem Standardwert **24** binden. Achte darauf, sinnvolle minimale und maximale Werte festzulegen.

## <a name="part-4-improve-the-zoom-experience"></a>Teil 4: Verbessern der Zoomfunktion

In diesem Abschnitt fügst du dem CodeBehind eine benutzerdefinierte `ItemSize`-Eigenschaft hinzu und erstellst unidirektionale Bindungen von der Bildvorlage zur neuen Eigenschaft. Der Wert `ItemSize` wird vom Schieberegler zum Zoomen und anderen Faktoren wie der Umschaltfläche **An Bildschirmgröße anpassen** und der Fenstergröße aktualisiert, sodass die Oberfläche genauer abgestimmt ist.

Anders als integrierte Eigenschaften von Steuerelementen aktualisieren deine benutzerdefinierten Eigenschaften nicht automatisch die Benutzeroberfläche, auch nicht bei unidirektionalen und bidirektionalen Bindungen. Sie funktionieren gut mit *einmaligen* Bindungen, aber wenn deine Änderungen auch auf der Benutzeroberfläche angezeigt werden sollen, musst du zuerst einige Aufgaben ausführen.

### <a name="create-the-itemsize-property-so-that-it-updates-the-ui"></a>Erstellen der ItemSize-Eigenschaft zum Aktualisieren der Benutzeroberfläche

1. Ändere in „MainPage.xaml.cs“ die Signatur der `MainPage`-Klasse, sodass sie die Schnittstelle `INotifyPropertyChanged` implementiert.

    **Vorher:**

    ```csharp
    public sealed partial class MainPage : Page
    ```

    **Nachher:**

    ```csharp
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    Dies informiert das Bindungssystem, das `MainPage` ein `PropertyChanged`-Ereignis aufweist (im nächsten Schritt hinzugefügt), auf das Bindungen lauschen können, um die Benutzeroberfläche zu aktualisieren.

2. Füge der `MainPage`-Klasse ein `PropertyChanged`-Ereignis hinzu.

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    Dieses Ereignis bietet die vollständige Implementierung, die für die Schnittstelle `INotifyPropertyChanged` benötigt wird. Damit es wirksam wird, musst du das Ereignis jedoch explizit in deinen benutzerdefinierten Eigenschaften auslösen.

3. Füge eine `ItemSize`-Eigenschaft hinzu, und lege das `PropertyChanged`-Ereignis in seinem Setter fest.

    ```csharp
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    Die `ItemSize`-Eigenschaft stellt den Wert des privaten Felds `_itemSize` zur Verfügung. Durch Verwenden eines solchen Hintergrundfelds kann die Eigenschaft überprüfen, ob ein neuer Wert identisch mit dem alten Wert ist, bevor sie ein möglicherweise unnötiges `PropertyChanged`-Ereignis auslöst.

    Das Ereignis selbst wird durch die `Invoke`-Methode ausgelöst. Das Fragezeichen überprüft, ob das `PropertyChanged`-Ereignis NULL ist, d. h., ob bereits ein Ereignishandler hinzugefügt wurde. Jede unidirektionale oder bidirektionale Bindung fügt im Hintergrund einen Ereignishandler hinzu, aber wenn nicht auf diesen gelauscht wird, geschieht nichts weiter. Wenn `PropertyChanged` nicht NULL ist, wird `Invoke` mit einem Verweis auf die Ereignisquelle (die Seite selbst, durch das Schlüsselwort `this` dargestellt) und einem **Ereignisargument**-Objekt, das den Namen der Eigenschaft angibt, aufgerufen. Mit diesen Informationen werden etwaige unidirektionale oder bidirektionale Bindungen für die `ItemSize`-Eigenschaft über Änderungen informiert, sodass die gebundene Benutzeroberfläche aktualisiert werden kann.

4. Suche in „MainPage.xaml“ eine `DataTemplate` mit dem Namen `ImageGridView_DefaultItemTemplate`, und ersetze die Werte `Height` und `Width` des Steuerelements `Grid` im oberen Bereich der Vorlage. (Wenn du im vorherigen Teil dieses Tutorials die Bindung von einem Steuerelement zum anderen erstellt hast, musst du lediglich noch `Value` durch `ItemSize` und `ZoomSlider` durch `page` ersetzen. Stellen Sie sicher, dass dies sowohl für `Height` als auch für `Width` ausgeführt wird!)

    **Vorher**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```

    **Nachher**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Da die Benutzeroberfläche auf Änderung von `ItemSize` reagieren kann, musst du tatsächlich einige Änderungen vornehmen. Wie bereits erwähnt, wird der Wert `ItemSize` vom aktuellen Status der verschiedenen Steuerelemente der Benutzeroberfläche berechnet, diese Berechnung muss jedoch ausgeführt werden, wenn sich der Zustand dieser Steuerelemente ändert. Zu diesem Zweck kannst du die Ereignisbindung verwenden, damit bestimmte Änderungen der Benutzeroberfläche eine Hilfsmethode aufrufen, die `ItemSize` aktualisiert.

### <a name="update-the-itemsize-property-value"></a>Aktualisieren des Werts der ItemSize-Eigenschaft

1. Füge „MainPage.xaml.cs“ die `DetermineItemSize`-Methode hinzu.

    ```csharp
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change,
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DefaultWindowSidePaddingValue"];
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);

            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);

            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. Navigiere in „MainPage.xaml“ an den Anfang der Datei, und füge dem Element `Page` die Ereignisbindung `SizeChanged` hinzu.

    **Vorher:**

    ```xaml
    <Page x:Name="page"
    ```

    **Nachher:**

    ```xaml
    <Page x:Name="page"
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. Suchen Sie den `Slider` mit dem Namen `ZoomSlider` (im Abschnitt `Page.Resources`), und fügen Sie eine `ValueChanged`-Ereignisbindung hinzu.

    **Vorher:**

    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **Nachher:**

    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. Suche `ToggleSwitch` mit dem Namen `FitScreenToggle`, und fügen die Ereignisbindung `Toggled` hinzu.

    **Vorher:**

    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **Nachher:**

    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

Führe die App aus, und ändere die Größe der Bildvorlage mit dem Schieberegler zum Zoomen und dem Umschalter **An Bildschirmgröße anpassen**. Wie du sehen kannst, ermöglichen die neuen Änderungen eine genauere Darstellung der Zoomgröße, während der Code gut organisiert bleibt.

![Ausführen der App mit aktivierter Anpassung an den Bildschirm](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> Versuche als Herausforderung, hinter dem `ZoomSlider` einen `TextBlock` hinzuzufügen und die `Text`-Eigenschaft an die `ItemSize`-Eigenschaft zu binden. Da es sich hier nicht um eine Datenvorlage handelt, kannst du `x:Bind` anstelle von `Binding` wie in den vorherigen `ItemSize`-Bindungen verwenden.

## <a name="part-5-enable-user-edits"></a>Teil 5: Aktivieren der Bearbeitung durch den Benutzer

Hier erstellst du bidirektionale Bindungen, damit Benutzer die Werte aktualisieren können, einschließlich des Bildtitels, der Bewertung und verschiedener visueller Effekte.

Dazu aktualisierst du die vorhandene `DetailPage`, die eine einzige Bildanzeige, ein Steuerelement zum Zoomen und eine Benutzeroberfläche zum Bearbeiten bereitstellt.

Zunächst musst du jedoch die `DetailPage` anhängen, sodass die App zu ihr navigiert, wenn der Benutzer in der Galerieansicht auf ein Bild klickt.

### <a name="attach-the-detailpage"></a>Anhängen der DetailPage

1. Suche in „MainPage.xaml“ die `GridView` mit dem Namen `ImageGridView`, und füge ein `ItemClick`-Attribut hinzu.

    > [!TIP]
    > Wenn Sie die nachstehende Änderung eingeben, anstatt sie zu kopieren und einzufügen, wird ein IntelliSense-Popup mit der Bezeichnung \<New Event Handler\> angezeigt. Wenn du die Tab-Taste drückst, wird der Wert mit einem Standardnamen für einen Methodenhandler ausgefüllt und die im nächsten Schritt dargestellte Methode automatisch ohne Funktion angelegt. Du kannst dann F12 drücken, um im CodeBehind zu der Methode zu navigieren.

    **Vorher:**

    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Nachher:**

    ```xaml
    <GridView x:Name="ImageGridView"
              ItemClick="ImageGridView_ItemClick"
    ```

    > [!NOTE]
    > Wir verwenden hier einen herkömmlichen Ereignishandler anstelle eines „x:Bind“-Ausdrucks. Dies liegt daran, dass wir die Ereignisdaten sehen müssen, wie nachstehend dargestellt.

2. Füge unter „MainPage.xaml.cs“ den Ereignishandler hinzu (oder fülle ihn aus, wenn du den Tipp im letzten Schritt verwendet hast).

    ```csharp
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    Diese Methode navigiert einfach zur Detailseite und übergibt dabei das Element, auf das geklickt wurde. Dabei handelt es sich um ein `ImageFileInfo`-Objekt, das von **DetailPage.OnNavigatedTo** zum Initialisieren der Seite verwendet wurde. Du musst diese Methode nicht in diesem Tutorial implementieren, aber du kannst dir ihre Funktion hier ansehen.

3. (Optional:) Lösche alle Steuerelemente, die du in vorherigen Schritten hinzugefügt hast und die mit dem aktuell ausgewählten Bild funktionieren, oder kommentiere sie aus. Sie beizubehalten, schadet zwar nicht, es ist nun jedoch erheblich schwieriger, ein Bild auszuwählen, ohne zur Detailseite zu navigieren.

Da du nun die beiden Seiten verbunden hast, führe die App aus, und probiere ihre Funktionen aus. Mit Ausnahme der Steuerelemente im Bearbeitungsbereich, die auf Versuche, ihre Werte zu ändern, nicht reagieren, funktioniert alles.

Wie du sehen kannst, wird im Textfeld der Titel angezeigt, und du kannst Änderungen eingeben. Du musst den Fokus auf ein anderes Steuerelement wechseln, um die Änderungen zu übernehmen, aber der Titel in der linken oberen Ecke des Bildschirms wird noch nicht aktualisiert.

Alle Steuerelemente sind bereits mithilfe der einfachen `x:Bind`-Ausdrücke aus Teil 1 gebunden. Wie zuvor erläutert, bedeutet dies, dass es sich durchgehend um einmalige Bindungen handelt – daher werden Änderungen an den Werten nicht registriert. Um dieses Problem zu beheben, müssen wir diese lediglich in bidirektionale Bindungen umwandeln.

### <a name="make-the-editing-controls-interactive"></a>Gestalten der Bearbeitungssteuerelemente als interaktiv

1. Suchen Sie in „DetailPage.xaml“ den `TextBlock` mit dem Namen **TitleTextBlock** und das Steuerelement **RatingControl** dahinter, und aktualisiere deren `x:Bind`-Ausdrücke, sodass sie **Mode=TwoWay** enthalten.

    **Vorher:**

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <muxc:RatingControl Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **Nachher:**

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}"
               ... >
    <muxc:RatingControl Value="{x:Bind item.ImageRating, Mode=TwoWay}"
                            ... >
    ```

2. Führe dieselben Schritte für alle Effektschieberegler durch, die dem Bewertungssteuerelement folgen.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ...
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ...
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ...
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ...
    ```

Der bidirektionale Modus bedeutet, dass Datenbewegungen in beiden Richtungen auftreten, wenn Änderungen auf einer Seite vorgenommen werden.

Wie die zuvor behandelten unidirektionalen Bindungen aktualisieren diese bidirektionalen Bindungen jetzt die Benutzeroberfläche, wenn die gebundenen Eigenschaften geändert werden. Dies ist auf die Implementierung von `INotifyPropertyChanged` in der `ImageFileInfo`-Klasse zurückzuführen. Mit bidirektionaler Bindung werden jedoch die Werte auch von der Benutzeroberfläche zu den gebundenen Eigenschaften verschoben, wenn der Benutzer mit dem Steuerelement interagiert. Auf XAML-Seite ist nichts weiter erforderlich.

Führe die App aus, und teste die Steuerelemente zum Bearbeiten. Wenn du eine Änderung vornimmst, wirkt sich dies nun auf die Bildwerte aus, und die Änderungen werden beibehalten, wenn du wieder zur Hauptseite navigierst.

## <a name="part-6-format-values-through-function-binding"></a>Teil 6: Formatieren von Werten über Funktionsbindungen

Ein letztes Problem bleibt bestehen. Wenn du die Effektschieberegler verschiebst, werden die Beschriftungen daneben nicht geändert.

![Effektschieberegler mit Standardbezeichnungswerten](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

Der letzte Teil dieses Tutorials behandelt das Hinzufügen von Bindungen, die die Werte des Schiebereglers für die Anzeige formatieren.

### <a name="bind-the-effect-slider-labels-and-format-the-values-for-display"></a>Binden der Effektschieberegler-Beschriftungen und Formatieren der Werte für die Anzeige

1. Suche den `TextBlock` hinter dem Schieberegler `Exposure`, und ersetze den Wert `Text` durch den hier gezeigten Bindungsausdruck.

    **Vorher:**

    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **Nachher:**

    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Dies wird als Funktionsbindung bezeichnet, da du eine Bindung an den Rückgabewert einer Methode erstellst. Die Methode muss über den CodeBehind der Seite oder den Typ `x:DataType` zugänglich sein, wenn du in einer Datenvorlage arbeitest. In diesem Fall ist die Methode die vertraute `ToString`-Methode von .NET. Es wird über die Elementeigenschaft der Seite und dann über die `Exposure`-Eigenschaft des Elements auf sie zugegriffen. (Dies veranschaulicht, wie du eine Bindung an Methoden und Eigenschaften erstellen kannst, die tief in einer Kette von Verbindungen verschachtelt sind.)

    Funktionsbindung ist eine ideale Möglichkeit zum Formatieren von Werten für die Anzeige, da andere Bindungsquellen als Methodenargumente übergeben werden können. Der Bindungsausdruck lauscht auf Änderungen an diesen Werten, wie es beim unidirektionalen Modus erwartet wird. In diesem Beispiel ist das **culture**-Argument ein Verweis auf ein unveränderliches Feld, das im CodeBehind implementiert ist, es könnte sich jedoch ebenso um eine Eigenschaft handeln, die `PropertyChanged`-Ereignisse auslöst. In diesem Fall würden Änderungen am Eigenschaftswert dazu führen, dass der `x:Bind`-Ausdruck `ToString` mit dem neuen Wert aufruft und dann die Benutzeroberfläche mit dem Ergebnis aktualisiert.

2. Führen Sie dies ebenso für die `TextBlock`s durch, die die Bezeichnungen der anderen Effektschieberegler darstellen.

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

Wenn du nun die App ausführst, funktioniert alles, einschließlich der Schiebereglerbezeichnungen.

![Effektschieberegler mit funktionierenden Bezeichnungen](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial hast du einen Einblick in die Datenbindung erhalten und etwas über einige der verfügbaren Funktionen erfahren. Eines ist vor dem Abschluss noch zu bedenken: Nicht alle Funktionen können gebunden werden, und manchmal sind die Werte, mit denen eine Verbindung hergestellt werden soll, mit den zu bindenden Eigenschaften nicht kompatibel. Es gibt zwar ein hohes Maß an Flexibilität beim Binden, es funktioniert jedoch nicht in jeder Situation.

Ein Beispiel für ein Problem, das nicht durch Bindungen gelöst werden kann, tritt auf, wenn ein Steuerelement keine geeigneten Eigenschaften zum Binden aufweist, wie die Detailseite der Funktion zum Zoomen. Dieser Schieberegler zum Zoomen muss mit dem `ScrollViewer` interagieren, der das Bild anzeigt, aber `ScrollViewer` kann nur über die eigene `ChangeView`-Methode aktualisiert werden. In diesem Fall verwenden wir konventionelle Ereignishandler, um die `ScrollViewer` und den Zoomschieberegler synchron zu halten. Ausführliche Informationen finden Sie unter den `ZoomSlider_ValueChanged`- und `MainImageScroll_ViewChanged`-Methoden in `DetailPage`.

Trotzdem ist das Binden eine leistungsfähige und flexible Möglichkeit, den Code zu vereinfachen und die Logik der Benutzeroberfläche von der der Daten getrennt zu halten. Dadurch ist es für Sie viel einfacher, eine Seite dieses Konzepts anzupassen und dabei das Risiko der Einführung von Fehlern auf der anderen Seite zu verringern.

Ein Beispiel der Trennung von Benutzeroberfläche und Daten ist die `ImageFileInfo.ImageTitle`-Eigenschaft. Diese Eigenschaft (ebenso wie die `ImageRating`-Eigenschaft) unterscheidet sich etwas von der `ItemSize`-Eigenschaft in Teil 4, da der Wert in den Dateimetadaten (über den Typ `ImageProperties`) gespeichert wird und nicht in einem Feld. Darüber hinaus gibt `ImageTitle` den Wert `ImageName` zurück (der auf den Namen der Datei festgelegt ist), wenn in den Metadaten der Datei kein Titel vorhanden ist.

```csharp
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

Wie du sehen kannst, aktualisiert der Setter die `ImageProperties.Title`-Eigenschaft und ruft dann `SavePropertiesAsync` auf, um den neuen Wert in die Datei zu schreiben. (Hierbei handelt es sich um eine asynchrone Methode, aber wir können das Schlüsselwort `await` nicht in einer Eigenschaft verwenden. Dies wäre auch nicht wünschenswert, da Getter und Setter sofort abgeschlossen werden würden. Stattdessen rufen Sie die Methode auf und ignorieren das zurückgegebene `Task`-Objekt.)

## <a name="going-further"></a>Vertiefung

Nach Abschluss dieser Übung verfügst du über ausreichende Kenntnisse zu Bindungen, um dich selbstständig mit Problemen auseinandersetzen zu können.

Möglicherweise ist dir aufgefallen: Wenn du den Zoomfaktor auf der Detailseite änderst, wird er automatisch zurückgesetzt, wenn du zurück navigierst und dasselbe Bild erneut aktivierst. Kannst du herausfinden, wie der Zoomfaktor für jedes Bild einzeln erhalten und wiederhergestellt werden kann? Viel Glück!

Du solltest in diesem Tutorial alle benötigten Informationen erhalten haben, aber wenn du weitere Unterstützung benötigst, erreichst du die Dokumente zur Datenbindung mit wenigen Mausklicks. Beginne hier:

+ [Markuperweiterung {x:Bind}](../xaml-platform/x-bind-markup-extension.md)
+ [Datenbindung im Detail](./data-binding-in-depth.md)