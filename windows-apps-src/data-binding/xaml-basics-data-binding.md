---
title: Erstellen von Datenbindungen
description: In diesem Artikel werden die Grundlagen der Datenbindung in XAML behandelt
keywords: XAML, UWP, Erste Schritte
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 21a053934d7391d12f7cd987026524b9ff4c279d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639985"
---
# <a name="create-data-bindings"></a>Erstellen von Datenbindungen

Angenommen, Sie haben eine schöne UI entworfen und implementiert, die mit Platzhalterbildern, "Lorem Ipsum"-Standardtext und Steuerelementen gefüllt ist, die noch keine Funktion haben. Als Nächstes möchten Sie es von einem Prototyp-Entwurf in eine echten App umwandeln und mit echte Daten verbinden. 

In diesem Lernprogramm erfahren Sie, wie Sie Ihre Textbausteine durch Datenbindungen ersetzen und andere direkte Links zwischen der Benutzeroberfläche und Ihren Daten erstellen. Außerdem erfahren, wie formatieren oder konvertieren Sie die Daten für die Anzeige, und Ihre Benutzeroberfläche und die Daten synchron halten. Wenn Sie dieses Tutorial abgeschlossen haben, Sie werden zur Verbesserung der Einfachheit und Organisation von der XAML und C# Codes, wodurch es einfacher zu verwalten und zu erweitern.

Sie beginnen mit einer vereinfachten Version des PhotoLab-Beispiels. Dieses Starterversion umfasst die vollständigen Datenebene sowie ein grundlegendes XAML-Layout und lässt viele kleinere Features aus, um den Code leichter zu durchsuchen. In diesem Lernprogramm lernen Sie nicht das Erstellen der vollständigen App. Lesen Sie daher die endgültige Version zu Features durch wie z. B. benutzerdefinierte Animationen und Support für Ihre Telefon. Die endgültige Version finden Sie im Stammordner des [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)-Repos. 

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017 und die neueste Version des Windows 10-SDKs](https://developer.microsoft.com/windows/downloads).

## <a name="part-0-get-the-code"></a>Teil 0: Code abrufen
Der Startpunkt für diese Übung befindet sich im PhotoLab-Beispiel-Repository, im Ordner [xaml-basics-starting-points/data-binding](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/data-binding). Nachdem Sie das Repository geklont/heruntergeladen haben, können Sie das Projekt bearbeiten, indem Sie PhotoLab.sln mit Visual Studio 2017 öffnen.

Die PhotoLab-App besteht aus zwei Hauptseiten:

**MainPage.xaml:** zeigt eine Ansicht der Foto-Galerie, zusammen mit einigen Informationen über jede Bilddatei an.
!["MainPage"](../design/basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.
![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="part-1-replace-the-placeholders"></a>Teil 1: Ersetzen Sie die Platzhalter

Hier erstellen Sie einmalige Bindungen der XAML-Datenvorlage, um echte Bilder und Metadaten von Bildern anstelle der Platzhalterinhalte anzuzeigen. 

Einmalige Bindungen eignen sich für schreibgeschützte, nicht veränderliche Daten d. h. sie sind sehr leistungsfähig und leicht zu erstellen und zeigen große Datensätze in den **GridView**- und **ListView**-Steuerelementen an. 

**Ersetzen Sie die Platzhalter durch einmalige Bindungen**

1. Öffnen Sie den Ordner „xaml-basics-startpunkte\data-binding” und starten Sie die Datei PhotoLab.sln. 

2. Stellen Sie sicher, dass Ihre Projektmappen-Plattform auf x86 oder x64 und nicht ARM festgelegt ist und führen Sie die App aus. Dies zeigt den Status der App mit UI-Platzhaltern an, bevor Bindungen hinzugefügt wurden. 

    ![Ausführen der App mit Platzhalterbildern und Text](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. Öffnen Sie MainPage.xaml und suchen Sie nach einem **DataTemplate** mit dem Namen **ImageGridView_DefaultItemTemplate**. Aktualisieren Sie diese Vorlage, um Datenbindungen zu verwenden. 

    **Vorher:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    Der Wert **x: Key** wird von **ImageGridView** verwendet, um diese Vorlage zum Anzeigen von Datenobjekten auszuwählen. 

4. Fügen Sie der Vorlage einen Wert **x: DataType** hinzu. 

    **Nachher:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
    ```

    **x: DataType** gibt an, für welchen Typ dies eine Vorlage ist. In diesem Fall ist es eine Vorlage für die **ImageFileInfo**-Klasse (wobei "lokal:" den lokalen Namespace gemäß der Definition in einer xmlns-Deklaration im oberen Bereich der Datei angibt).
    
    **x: DataType** wird benötigt, wenn **x: Bind**-Ausdrücke in einer Datenvorlage verwendet werden, wie nachfolgend beschrieben. 

5. Suchen Sie in **DataTemplate** das Element **Image** mit dem Namen **ItemImage** und ersetzen Sie seinen **Quell**-Wert wie hier gezeigt. 

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
    
    **x: Name** identifiziert ein XAML-Element, damit Sie darauf an anderer Stelle im XAML-Code und im CodeBehind verweisen können. 

    **x: Bind**-Ausdrücke geben der UI-Eigenschaft einen Wert, indem es einen Wert von einer **Datenobjekt**-Eigenschaft abruft. In Vorlagen ist die angegebene Eigenschaft eine Eigenschaft, die im **x: DataType** festgelegt wurde. In diesem Fall ist die Datenquelle also die **ImageFileInfo.ImageSource**-Eigenschaft. 
    
    > [!NOTE] 
    > Der **x: Bind**-Wert informiert ebenfalls den Editor über den Datentyp, damit Sie IntelliSense anstatt verwenden können, anstatt den Namen der Eigenschaft in den **X: Bind**-Ausdruck einzugeben. Versuchen Sie diesen Vorgang auf dem gerade eingefügten Code durchzuführen: platzieren Sie den Cursor unmittelbar nach **x: Bind** und drücken Sie die LEERTASTE, um eine Liste der Eigenschaften anzuzeigen, für die Sie eine Bindung erstellen können.

6. Ersetzen Sie die Werte der anderen UI-Steuerelemente auf die gleiche Weise. (Probieren Sie es mithilfe von IntelliSense anstatt Kopieren/Einfügen!)

    **Vorher:**
    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <telerikInput:RadRating Value="3" ... />
    ```
    
    **Nachher:**
    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <telerikInput:RadRating Value="{x:Bind ImageRating}" ... />
    ```

Führen Sie die App aus, um herauszufinden, wie sie bisher aussieht. Keine weiteren Platzhalter! Das ist ein guter Ausgangspunkt. 

![Die App mit richtigen Bildern und Text anstelle von Platzhaltern ausführen](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> Wenn Sie weiter experimentieren möchten, versuchen Sie, der Datenvorlage einen neuen TextBlock hinzuzufügen, und verwenden Sie den x: Bind IntelliSense-Trick, um eine Eigenschaft zu suchen, die Sie anzeigen möchten. 

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>Teil 2: Verwenden Sie die Bindung zur Verbindung mit den Images der Benutzeroberfläche des Katalogs

Hier erstellen Sie einmalige Bindungen auf XAML-Seiten für die Verbindung der Fotogalerie-Ansicht mit der Bildersammlung und ersetzen dadurch den vorhandenen prozeduralen Code, der dies im CodeBehind durchführt. Erstellen Sie außerdem eine Schaltfläche **Löschen**, um zu sehen, wie sich die Fotogalerie-Ansicht ändert, wenn Sie Bilder aus der Sammlung entfernen. Gleichzeitig erfahren Sie, wie Ereignisse für mehr Flexibilität als herkömmliche Ereignishandler an Ereignishandler gebunden werden. 

Alle Bindungen, die bisher behandelt wurden, befinden sich innerhalb der Datenvorlagen und beziehen sich auf Eigenschaften der Klasse, die vom Wert **x: DataType** angegeben werden. Was ist mit den Rest des XAML-Codes auf der Seite? 

**x: Bind**-Ausdrücke außerhalb von Datenvorlagen sind immer an die Seite selbst gebunden. Dies bedeutet, dass Sie auf alles im CodeBehind oder auf XAML verweisen können, einschließlich der benutzerdefinierten Eigenschaften und der Eigenschaften anderer UI-Steuerelemente auf der Seite (solange sie einen Wert **x: Name** haben). 

Im PhotoLab-Beispiel kann eine solche Bindung verwendet werden, um das Haupt-**GridView**-Steuerelement direkt mit der Sammlung von Bildern zu verbinden, anstatt dies im CodeBehind durchzuführen. Später sehen Sie weitere Beispiele. 

**Binden Sie die wichtigsten GridView-Steuerelement an die Auflistung der Bilder**

1. Suchen Sie unter MainPage.xaml.cs die **OnNavigatedTo**-Methode und entfernen Sie den Code, der **ItemsSource**  festlegt.

    **Vorher:**
    ```c#
    ImageGridView.ItemsSource = Images;
    ```

    **Nachher:**
    ```c#
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. Suchen Sie in MainPage.xaml das **GridView** mit dem Namen **ImageGridView** und fügen Sie ein **ItemsSource**-Attribut hinzu. Verwenden Sie für den Wert einen **X: Bind**-Ausdruck, der sich auf die **Bild**-Eigenschaft bezieht, die im CodeBehind implementiert wurde. 

    **Vorher:**
    ```xaml
    <GridView x:Name="ImageGridView" 
    ```

    **Nachher:**
    ```xaml
    <GridView x:Name="ImageGridView" 
              ItemsSource="{x:Bind Images}" 
    ```

    Die **Images** Eigenschaft ist vom Typ **ObservableCollection\<ImageFileInfo\>**, damit die einzelnen Elemente der Anzeige der **GridView** sind Der Typ **ImageFileInfo**. Dies entspricht dem **x: DataType**-Wert, der in Teil 1 beschrieben wurde. 

Alle Bindungen, die wir haben bisher untersucht haben, sind einmalige, schreibgeschützte Bindungen und entsprechen dem Standardverhalten für nur **x: Bind**-Ausdrücke. Die Daten werden nur bei der Initialisierung hochgeladen, wodurch High-End-Bindungen entstehen – dies eignet sich ideal für die Unterstützung von mehreren komplexen Ansichten großer Datensätze. 

Sogar die **ItemsSource**-Bindung, die Sie gerade hinzugefügt haben ist nur eine einmalige, schreibgeschützte Bindung für einen statischen Wert der Eigenschaft, aber hier gibt es einen wichtigen Unterschied. Der sich nicht ändernde Wert der **Bild**-Eigenschaft ist eine einzelne, spezifische Instanz einer Sammlung, die einmal, wie hier gezeigt, initialisiert wurde.

```Csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

Die **Images** Eigenschaftswert sich nie ändert, aber da die Eigenschaft vom Typ **ObservableCollection\<T\>**, *Inhalt* von der Auflistung kann geändert werden, und die Bindung automatisch Beachten Sie die Änderungen und die Benutzeroberfläche aktualisiert wird. 

Um dies zu testen, werden wir vorübergehend eine Schaltfläche hinzufügen, die das derzeit ausgewählte Bild löscht. In der endgültigen Version ist diese Schaltfläche nicht vorhanden, da die Auswahl eines Bildes Sie zu einer Detailseite führt. Jedoch das Verhalten der **ObservableCollection\<T\>**  ist im letzten Beispiel PhotoLab wichtig, da die XAML in der Page-Konstruktor initialisiert wird (über die  **InitializeComponent** Methodenaufruf), aber die **Images** -Auflistung wird aufgefüllt, weiter unten in der **OnNavigatedTo** Methode. 

**Hinzufügen einer Schaltfläche "löschen"**

1. Suchen Sie in MainPage.xaml die **CommandBar** mit dem Namen **MainCommandBar** und fügen Sie eine neue Schaltfläche vor der Schaltfläche zum Zoomen hinzu. (Die Zoomsteuerelemente funktionieren noch nicht. Sie können diese im nächsten Teil des Lernprogramms anschließen.)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    Wenn Sie bereits mit XAML vertraut sind, sieht dieser **Klick**-Wert möglicherweise ungewöhnlich aus. In früheren Versionen von XAML mussten Sie dies zu einer Methode mit einer bestimmten Ereignishandler-Signatur hinzufügen, einschließlich der Parameter für den Absender des Ereignisses und eines ereignisspezifischen Argumentobjekts. Sie können diese Technik auch weiterhin verwenden, wenn Sie Ereignisargumente benötigen, aber mit x: Bind können Sie keine Verbindung zu anderen Methoden herstellen. Wenn Sie beispielsweise die Ereignisdaten nicht benötigen, können Sie keine Verbindung zu Methoden herstellen, die keine Parameter haben, wie hier gezeigt.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. Fügen Sie in MainPage.xaml.cs die **DeleteSelectedImage**-Methode hinzu.

    ```c#
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    Diese Methode löscht das ausgewählte Bild aus der **Bild**-Sammlung. 

Führen Sie jetzt die App aus und verwenden Sie die Schaltfläche, um ein paar Bilder zu löschen. Wie Sie sehen können, die Benutzeroberfläche aufgrund der Datenbindung automatisch aktualisiert wird und die **ObservableCollection\<T\>**  Typ. 

> [!Note]
> Hier ist eine schwierigere Herausforderung: versuchen Sie, zwei Schaltflächen hinzuzufügen, die das ausgewählte Bild in der Liste nach oben oder unten verschieben, und binden Sie dann das x: Bind-Klick-Ereignisse dieser zwei neuen Methoden an DeleteSelectedImage.
 
## <a name="part-3-set-up-the-zoom-slider"></a>Teil 3: Richten Sie den Zoomschieberegler 

In dieser Phase erstellen wir unidirektionale Bindungen von einem Steuerelement in der Datenvorlage zum Zoomschieberegler, der sich außerhalb der Vorlage befindet. Außerdem erfahren Sie, dass Sie die Datenbindung mit vielen Eigenschaften des Steuerelements verwenden können, nicht nur die offensichtlichsten, wie **TextBlock.Text** und **Image.Source**. 

**Binden Sie die Vorlage für Images Daten an der Zoomschieberegler**

* Suchen Sie ein **DataTemplate** mit dem Namen **ImageGridView_DefaultItemTemplate** und ersetzen Sie die Werte von **Höhe** und **Breite** des **Raster**-Steuerelements im oberen Bereich der Vorlage.

    **Vor dem**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Nach dem**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    <!-- TODO talk about dependency properties --> 
    
    Haben Sie bemerkt, dass dies **Bindungs**-Ausdrücke sind und nicht **x: Bind**-Ausdrücke? Dies ist die alte Vorgehensweise der Nutzung von Datenbindungen, und diese ist veraltet. **x: Bind** hat fast alle Funktionen von **Bindung** und vieles mehr. Wenn Sie jedoch **x: Bind** in einer Datenvorlage verwenden, bindet sie sich an den Typ, der im Wert **x: DataType** deklariert ist. Wie binden Sie etwas in der Vorlage an etwas auf der XAML-Seite oder im CodeBehind? Müssen Sie einen veralteten **Bindungs**-Ausdruck verwenden. 
    
    **Bindungs**-Ausdrücke erkennen den **x: DataType**-Wert nicht an, aber diese **Bindungs**-Ausdrücken haben **ElementName**-Werte, die nahezu auf die gleiche Weise funktionieren. Diese teilen dem Bindungsmodul mit, dass **Binding Value** eine Bindung für die **Wert**-Eigenschaft für das angegebene Element auf der Seite ist (d. h. das Element mit dem **x: Name**-Wert). Wenn Sie eine Eigenschaft im CodeBehind binden möchten, würde es etwa so aussehen ```{Binding MyCodeBehindProperty, ElementName=page}``` wobei **Seite** sich auf den **x: Name**-Wert bezieht, der im Element **Seite** in XAML festgelegt ist. 
    
    > [!NOTE]
    > In der Standardeinstellung sind **Bindungs**-Ausdrücke *unilateral*, was bedeutet, dass sie die Benutzeroberfläche automatisch aktualisieren, wenn die Eigenschaft des gebundenen Werts sich ändert. 
    > 
    > Im Gegensatz dazu ist die Standardeinstellung für **x: Bind***einmalig*, was bedeutet, dass jegliche Änderungen an der gebundenen Eigenschaft ignoriert werden. Dies ist die Standardeinstellung, da es die leistungsstärkste Option ist und die meisten Bindungen zu statischen, schreibgeschützten Daten erstellt werden. 
    >
    > Hier gilt folgende Lektion: die bei Verwendung von **X: Bind** mit Eigenschaften, deren Werte sich ändern kann, müssen Sie ```Mode=OneWay```oder ```Mode=TwoWay``` hinzufügen. Im nächsten Abschnitt sehen Sie ein Beispiel dafür.

Führen Sie die App aus und verwenden Sie den Schieberegler, um die Größe der Image-Vorlage zu ändern. Wie Sie sehen können, ist der Effekt auch ohne viel Code imposant. 

![Die App mit angezeigtem Zoomschieberegler ausführen](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> Hier ist eine Herausforderung: versuchen Sie andere UI-Eigenschaften an die **Wert**-Eigenschaft des Zoomschiebereglers oder eines anderen Reglers zu binden, den Sie nach dem Zoomschieberegler hinzufügen. Sie können z. B. die **FontSize**-Eigenschaft der **TitleTextBlock** auf einen neuen Schieberegler mit einem Standardwert von **24** binden. Achten Sie darauf, dass Sie angemessene minimale und maximalen Werte festlegen.

## <a name="part-4-improve-the-zoom-experience"></a>Teil 4: Verbessern Sie die benutzerfreundlichkeit für zoom 

In diesem Abschnitt fügen Sie dem CodeBehind eine benutzerdefinierte **ItemSize**-Eigenschaft hinzu und erstellen eine unidirektionale Bindungen von der Bildvorlage auf die neue Eigenschaft. Der **ItemSize**-Wert wird vom Zoomschieberegler und anderen Faktoren wie z. B. dem Umschalten von **Bildschirmgröße anpassen** und der Fenstergröße aktualisiert, die so eine präzisere Erfahrung bieten. 

Im Gegensatz zu integrierten Steuerelement-Eigenschaften werden Ihre benutzerdefinierten Eigenschaften nicht automatisch die Benutzeroberfläche aktualisieren, auch nicht bei unidirektionale und bidirektionale Bindungen. Sie funktionieren gut **einmaligen** Bindungen, aber wenn Sie Ihre Änderungen tatsächlich auf der Benutzeroberfläche anzeigen möchten, müssen Sie zuerst einige Aufgaben ausführen. 

**Erstellen Sie die ItemSize-Eigenschaft, sodass die Benutzeroberfläche aktualisiert**

1. In MainPage.xaml.cs, ändern Sie die Signatur der **MainPage**-Klasse, damit die **INotifyPropertyChanged**-Schnittstelle implementiert wird.

    **Vorher:**
    ```c#
    public sealed partial class MainPage : Page
    ```

    **Nachher:**
    ```c#
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    Dies informiert das Bindungssystem, das auf „MainPage” ein „PropertyChanged”-Ereignis durchgeführt wurde (wird als Nächstes hinzugefügt), auf das Bindungen horchen können, um die UI zu aktualisieren. 

2. Fügen Sie ein **PropertyChanged**-Ereignis der **MainPage**-Klasse hinzu.

    ```c#
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    Dieses Ereignis bietet die vollständige Implementierung, die von der **INotifyPropertyChanged**-Schnittstelle benötigt wird. Damit es wirksam wird, müssen Sie explizit das Ereignis in Ihren benutzerdefinierten Eigenschaften auslösen. 

3. Fügen Sie eine **ItemSize**-Eigenschaft hinzu und lösen Sie das **PropertyChanged**-Ereignis in seinem Setter.

    ```c#
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

    Die **ItemSize**-Eigenschaft enthält den Wert eines privaten **_itemSize**-Felds. Durch Verwenden eines solchen Sicherungsfelds kann die Eigenschaft überprüfen, ob ein neuer Wert identisch mit dem alten Wert ist, bevor es möglicherweise unnötig ein **PropertyChanged**-Ereignis auslöst.

    Das Ereignis selbst wird durch die **Invoke**-Methode ausgelöst. Das Fragezeichen überprüft, ob das **PropertyChanged**-Ereignis null ist – d. h., ob bereits ein Ereignishandler hinzugefügt wurde. Jede unidirektionale oder bidirektionale Bindung fügt einen Ereignishandler in den Hintergrund hinzu, aber wenn niemand horcht, geschieht hier nichts mehr. Wenn **PropertyChanged** nicht null ist, wird **Invoke** mit einem Verweis auf die Ereignisquelle (die Seite selbst, die durch das Schlagwort **dies** dargestellt wird) und einem **Ereignisargument**-Objekt aufgerufen , das den Namen der Eigenschaft anzeigt. Mit diesen Informationen, wird jede unidirektionale oder bidirektionalen Bindungen für die **ItemSize**-Eigenschaft über jegliche Änderungen informiert, damit die gebundene UI aktualisiert werden kann. 

4. Suchen Sie in MainPage.xaml ein **DataTemplate** mit dem Namen **ImageGridView_DefaultItemTemplate** und ersetzen Sie die Werte von **Höhe** und **Breite** des **Raster**-Steuerelements im oberen Bereich der Vorlage. (Wenn Sie im vorherigen Teil dieses Lernprogramms die Bindungen von einem Steuerelement zum anderen erstellt haben, müssen Sie als einzige Änderungen **Wert** durch **ItemSize** und **ZoomSlider** durch **Seite** ersetzen. Führen Sie dies für die Höhe und Breite aus!)

    **Vor dem**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Nach dem**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Da die Benutzeroberfläche auf die Änderung von **ItemSize** reagieren kann, müssen Sie tatsächlich einige Änderungen vornehmen. Wie bereits erwähnt, wird der **ItemSize**-Wert vom aktuellen Status der verschiedenen UI-Steuerelemente berechnet, aber die Berechnung muss ausgeführt werden, wenn sich der Zustand dieser Steuerelemente ändert. Zu diesem Zweck können Sie die Ereignisbindung verwenden, damit bestimmte UI-Änderungen eine Hilfsmethode aufrufen, die **ItemSize** aktualisiert. 

**Aktualisieren Sie den Wert der ItemSize-Eigenschaft**

1. Fügen Sie MainPage.xaml.cs die **DetermineItemSize**-Methode hinzu.

    ```c#
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
                (int)this.Resources["DesktopWindowSidePaddingValue"];
    
            if (AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile" &&
                UIViewSettings.GetForCurrentView().UserInteractionMode ==
                    UserInteractionMode.Touch)
            {
                margins = (int)this.Resources["SmallItemMarginValue"] * 4;
                gridWidth = ImageGridView.ActualWidth -
                    (int)this.Resources["MobileWindowSidePaddingValue"];
            }
    
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

2. Navigieren Sie in MainPage.xaml an den Anfang der Datei und fügen Sie eine **SizeChanged**-Ereignisbindung dem Element **Seite** hinzu.

    **Vorher:**
    ```xaml
    <Page x:Name="page"  
    ```

    **Nachher:**
    ```xaml
    <Page x:Name="page" 
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. Suchen Sie den **Schieberegler** mit dem Namen **ZoomSlider** und fügen Sie die **ValueChanged**-Ereignisbindung hinzu.

    **Vorher:**
    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **Nachher:**
    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. Suchen Sie den **ToggleSwitch** mit dem Namen **FitScreenToggle** und fügen Sie die **Toggled**-Ereignisbindung hinzu.

    **Vorher:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **Nachher:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

Führen Sie die App aus und verwenden Sie den Zoomschieberegler und den Schalter **Bildschirmgröße anpassen**, um die Größe des Image-Vorlage zu ändern. Wie Sie sehen können, ermöglichen die neuesten Änderungen eine genauere Erfahrung der Zoomgröße, während der Code gut organisiert bleibt. 

![Ausführen der App mit aktivierter Bildschirmanpassung](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> Hier ist eine Herausforderung: versuchen Sie einen **TextBlock** nach dem **ZoomSlider** hinzuzufügen und die **Text**-Eigenschaft an die **ItemSize**-Eigenschaft zu binden. Da es sich nicht um eine Datenvorlage handelt, können Sie **x: Bind** anstelle von **Bindung** wie in den vorherigen **ItemSize**-Bindungen verwenden.  
}

## <a name="part-5-enable-user-edits"></a>Teil 5: Aktivieren Sie die benutzerbearbeitungen

Hier erstellen Sie bidirektionale Bindungen, damit Benutzer die Werte aktualisieren können, einschließlich der Bildtitel, Bewertung und verschiedener visuelle Effekte. 

Um dies zu erreichen, aktualisieren Sie die vorhandene **DetailPage**, die ein einzige Bildanzeige, ein Zoom-Steuerelement und das Bearbeiten der Benutzeroberfläche ermöglicht.  

Zunächst müssen Sie jedoch die **DetailPage** anhängen, damit die App darauf navigiert, wenn der Benutzer ein Bild in der Galerie anklickt.

**Fügen Sie der DetailPage**

1. Suchen Sie in MainPage.xaml das **GridView** mit dem Namen **ImageGridView** und fügen Sie ein **ItemClick**-Attribut hinzu. 

    > [!TIP] 
    > Wenn Sie in die folgende Änderung anstelle von kopieren/einfügen eingeben, sehen Sie eine IntelliSense-Popupfenster, die besagt "\<neuen Ereignishandler\>". Wenn Sie die Tab-Taste drücken, wird es den Wert mit dem Namen eines standardmäßigen Ereignishandlers füllen und automatisch die Methode im nächsten Schritt auskommentieren. Drücken Sie dann F12, um zu der Methode im CodeBehind zu navigieren. 

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
    > Wir verwenden hier einen herkömmlichen Ereignishandler anstelle eines x: Bind-Ausdrucks. Dies liegt daran, dass wir die Ereignisdaten sehen müssen, wie nachfolgend dargestellt. 

2. Fügen Sie unter MainPage.xaml.cs den Ereignishandler hinzu (oder füllen Sie ihn aus, wenn Sie den Tipp im letzten Schritt verwendeten).

    ```c#
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    Diese Methode navigiert einfach auf die Detailseite, und übergibt das geklickt Element, was ein **ImageFileInfo**-Objekt ist, das von **DetailPage.OnNavigatedTo** zum Initialisieren der Seite verwendet wurde. Sie müssen diese Methode nicht in diesem Lernprogramm implementieren, aber Sie können hier sehen, was das Resultat ist. 
    
3. (Optional) Löschen Sie, oder kommentieren Sie alle Steuerelemente, die Sie in vorherigen Play-Punkten hinzugefügt haben und die mit dem aktuell ausgewählten Bild funktionieren. Das Beibehalten schadet zwar nicht, es ist macht es viel schwieriger, ein Bild auszuwählen, ohne zur Detailseite zu navigieren. 

Jetzt, wo Sie die beiden Seiten angeschlossen haben, führen Sie die App aus und schauen sie im Detail an. Alles funktioniert mit Ausnahme der Steuerelemente im Bereich „Bearbeiten”, der nicht reagieren, wenn Sie versuchen, die Werte zu ändern. 

Wie Sie sehen können, zeigt das Textfeld den Titel an und Sie können Änderungen eingeben. Sie müssen den Fokus auf ein anderes Steuerelement leiten, um die Änderungen zu übernehmen, aber der Titel in der oberen linken Ecke des Bildschirms kann noch nicht aktualisiert werden. 

Alle Steuerelemente sind bereits mithilfe der einfachen **x: Bind**-Ausdrücke gebunden, die wir in Teil 1 behandelt haben. Wenn Sie sich erinnern, bedeutet dies, dass sie alle einmalige Bindungen sind, was erklärt, warum die Änderungen der Werte nicht registriert sind. Um dieses Problem zu beheben, müssen wir diese lediglich in bidirektionalen Bindungen umwandeln. 

**Stellen Sie die Bearbeitungssteuerelementen interaktive**

1. Suchen Sie in DetailPage.xaml den **TextBlock** mit dem Namen **TitleTextBlock** und das **RadRating**-Steuerelement dahinter, und aktualisieren Sie deren **x: Bind**-Ausdrücke, damit sie **Mode = TwoWay** enthalten.

    **Vorher:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **Nachher:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock" 
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}" 
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating, Mode=TwoWay}" 
                            ... >
    ```

2. Führen Sie dieselben Schritte für den Effekt-Schieberegler durch, die dem Bewertungssteuerelement folgen.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ... 
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ... 
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ... 
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ... 
    ```

Der Zwei-Wege-Modus bedeutet, dass die Daten in beide Richtungen verschoben werden, wenn Änderungen auf beiden Seiten vorgenommen werden. 

Wie die unidirektionalen Bindungen, die wir vorher behandelt haben, werden diese bidirektionalen Bindungen jetzt die UI aktualisieren, wenn die gebundenen sich ändern. Dies geschieht dank der **INotifyPropertyChanged**-Implementierung in der **ImageFileInfo**-Klasse. Mit bidirektionaler Bindung werden jedoch die Werte auch von der Benutzeroberfläche auf die gebundenen Eigenschaften verschoben, wenn der Benutzer mit dem Steuerelement interagiert. Auf der XAML-Seite ist nichts weiter erforderlich. 

Führen Sie die App aus und testen Sie die Bearbeitungssteuerelemente. Wenn Sie eine Änderung vornehmen, wirkt sich dies jetzt auf den Bildwert aus, und diese Änderungen bleiben konstant, wenn Sie wieder zur Hauptseite navigieren. 

## <a name="part-6-format-values-through-function-binding"></a>Teil 6: Formatieren von Werten über funktionsbindung

Ein letztes Problem bleibt bestehen. Wenn Sie den Effekt-Schieberegler verschieben, ändern Sie die Beschriftungen daneben nicht. 

![Effekt-Schieberegler mit Standardbezeichnungswerten](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

Der letzte Teil dieses Lernprogramms behandelt das Hinzufügen von Bindungen, die die Werte des Schiebereglers für die Anzeige formatieren.

**Binden Sie die Auswirkungen schiebereglerbezeichnungen und formatieren Sie die Werte für die Anzeige**

1. Suchen der **TextBlock** nach der **Offenlegung** Schieberegler, und ersetzen die **Text** Wert mit den hier gezeigten Bindungsausdruck.

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

    Dies wird Funktions-Bindung genannt, da Sie eine an den Rückgabewert einer Methode binden. Die Methode muss über die Seite „CodeBehind” oder über den **x: DataType**-Typ zugänglich sein, wenn Sie in einer Datenvorlage arbeiten. In diesem Fall ist die Methode die vertraute .NET **ToString**-Methode, auf die über die Elementeigenschaft auf der Seite zugegriffen werden kann, sowie über die **Belichtungs**-Eigenschaft des Elements. (Dies veranschaulicht, wie Sie eine Bindung an die Methoden und Eigenschaften erstellen können, die in einer Kette von Verbindungen tief verschachtelt sind.)

    Funktions-Bindung ist eine ideale Möglichkeit zur Formatierung von Werten für die Anzeige, da andere Bindungsquellen als Methodenargumente übergeben werden können und der Bindungsausdruck die Änderungen für diese Werte abhört, wie bei dem unidirektionalen Modus erwartet wird. In diesem Beispiel ist das **Kultur**-Argument ein Verweis auf ein sich nicht änderndes Feld, das im CodeBehind implementiert ist, aber es könnte genauso einfach eine Eigenschaft sein, die **PropertyChanged**-Ereignisse auslöst. In diesem Fall würden jegliche Änderungen an den Wert der Eigenschaft den **X: Bind**-Ausdruck zum Aufrufen von **ToString** mit dem neuen Wert auslösen und dann die Benutzeroberfläche mit dem Ergebnis aktualisieren. 

2. Führen Sie dieselben Schritte für die **TextBlocks**-Bezeichnungen durch, die die anderen Effekt-Schieberegler beschriften.

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

Wenn Sie die App ausführen, funktioniert jetzt alles, einschließlich der Schiebereglerbeschriftungen. 

![Effekt-Schieberegler mit Funktionsbezeichnungen](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

> [!NOTE]
> Versuchen Sie die Funktions-Bindung mit dem **TextBlock** vom letzten Play-Punkt und binden Sie sie an eine neue Methode, die eine Zeichenfolge im "000 x 000"-Format zurückgibt, wenn Sie sie an den **ItemSize**-Wert übergeben.


## <a name="conclusion"></a>Abschluss

In diesem Lernprogramm haben Sie einen Einblick in die Datenbindung und einige der verfügbaren Funktionen erhalten. Eines möchten wir jedoch zu bedenken geben: nicht alles kann gebunden werden, und in einigen Fällen sind die Werte zum Herstellen einer Verbindung nicht kompatibel mit den Eigenschaften, die Sie anbinden möchten. Es gibt zwar ein hohes Maß an Flexibilität beim Binden, dies funktioniert allerdings nicht in jeder Situation.

Ein Beispiel für ein Problem, das nicht durch Bindungen angesprochen wird ist, wenn ein Steuerelement keine geeigneten Eigenschaften zum Binden hat, wie bei der Detailseite der Zoom-Funktion. Dieser Zoomschieberegler muss mit **ScrollViewer** interagieren, das das Bild anzeigt, aber **ScrollViewer** kann nur über die **ChangeView**-Methode aktualisiert werden. In diesem Fall verwenden wir konventionellen Ereignishandler zu der **ScrollViewer** und Zoomschieberegler in synchronisieren, finden Sie unter den **DetailPage** **ZoomSlider_ValueChanged** und **MainImageScroll_ViewChanged** Methoden für die Details.

Trotzdem ist das Binden eine leistungsfähige und flexible Möglichkeit zur Vereinfachung des Codes und um die UI-Logik von der Datenlogik getrennt zu halten. Dadurch ist es viel einfacher für Sie, beide Seiten zu koordinieren und gleichzeitig das Risikos der Einführung von Fehlern auf der anderen Seite zu reduzieren. 

Ein Beispiel der Trennung von Benutzeroberfläche und Daten ist die **ImageFileInfo.ImageTitle**-Eigenschaft. Diese Eigenschaft (und die **ImageRating**-Eigenschaft) unterscheidet sich etwas von der **ItemSize**-Eigenschaft im Teil 4, da der Wert in den Dateimetadaten statt in einem Feld gespeichert wird (über den **ImageProperties**-Typ). Darüber hinaus gibt der **ImageTitle** den **ImageName**-Wert zurück (auf den Namen der Datei festgelegt), wenn kein Titel in den Metadaten der Datei vorhanden ist. 

```c#
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

Wie Sie sehen können, aktualisiert der Setter die **ImageProperties.Title**-Eigenschaft und ruft dann **SavePropertiesAsync** auf, um den neuen Wert in die Datei zu schreiben. (Hierbei handelt es sich um eine asynchrone Methode, aber wir können nicht das **await**-Schlüsselwort in einer Eigenschaft verwenden – was nicht wünschenswert wäre, da das Erstellen und Abrufen von Eigenschaften sofort ausgeführt würde. Sie würden stattdessen die Methode aufrufen und die das zurückgegebene **Aufgabe**-Objekt ignorieren.) 

<!-- TODO more screenshots? -->

## <a name="going-further"></a>Vertiefung

Nach Abschluss dieser Übung verfügen Sie über ausreichende adaptive Bindungs-Kenntnisse, um selbst weiter zu experimentieren.

Wie Sie gesehen haben, wenn Sie den Zoomfaktor auf der Detailseite ändern, wird er automatisch zurückgesetzt, wenn Sie rückwärts navigieren und das gleiche Bild erneut aktivieren. Können Sie herausfinden, wie Sie den Zoomfaktor für jedes Bild einzeln erhalten und wieder herstellen? Viel Glück!
    
Sie sollten in diesem Lernprogramm alle benötigten Informationen erhalten haben, aber wenn Sie weitere Unterstützung benötigen, sind die Datenbindungsdokumente nur einen Mausklick entfernt. Beginnen Sie hier:

+ [{X: Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
+ [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)