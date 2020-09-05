---
title: Erstellen benutzerdefinierter Stile
description: In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Stile und Schieberegler erstellen, um die Benutzeroberfläche Ihrer XAML-App anzupassen.
keywords: XAML, UWP, Erste Schritte
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6d3423e9d78e2519f2d3c9ad1fc2c0b099de0349
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160774"
---
# <a name="tutorial-create-custom-styles"></a>Tutorial: Erstellen benutzerdefinierter Stile

In diesem Tutorial wird die Anpassung der Benutzeroberfläche (User Interface, UI) unserer XAML-App veranschaulicht. Warnung: in diesem Tutorial taucht möglicherweise ein Einhorn auf. (Im Ernst!)

Die Beispiel-App für PhotoLab verfügt über zwei Seiten. Die _Hauptseite_ zeigt eine Ansicht der Foto-Galerie zusammen mit einigen Informationen über jede Bilddatei an.

![MainPage](../basics/images/xaml-basics/mainpage.png)

Die *Detailseite* zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Voraussetzungen

+ Visual Studio 2019: [Visual Studio 2019 herunterladen](https://visualstudio.microsoft.com/downloads/) (Community-Edition ist kostenlos)
+ Windows 10 SDK (10.0.17763.0 oder höher):  [Das aktuelle Windows SDK herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, Version 1809 oder höher

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Startcode von GitHub abrufen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels.

1. Zur GitHub-Seite für das Beispiel navigieren: [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
2. Als Nächstes müssen Sie das Beispiel klonen oder herunterladen. Wähle die Schaltfläche **Clone or download** (Klonen oder Herunterladen) aus. Ein Untermenü erscheint.
    ![Das Menü „Clone or download“ (Klonen oder Herunterladen) auf der GitHub-Seite des PhotoLab-Beispiels](images/xaml-basics/clone-repo.png)

    **Wenn Sie mit GitHub nicht vertraut sind:**

    ein. Wähle **Download ZIP** aus, und speichere die Datei lokal. Es wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.

    b. Entpacken Sie die Datei. Verwenden Sie den Datei-Explorer, um zu der soeben heruntergeladenen ZIP-Datei zu navigieren, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Alle extrahieren** aus.

    c. Navigiere zu deiner lokalen Kopie des Beispiels, und wechsle zum Verzeichnis `Windows-appsample-photo-lab-master\xaml-basics-starting-points\style`.

    **Wenn Sie mit GitHub vertraut sind:**

    ein. Klonen Sie den Master-Branch des Repositorys lokal.

    b. Wechsle zum `Windows-appsample-photo-lab\xaml-basics-starting-points\style`-Verzeichnis.

3. Doppelklicken Sie auf `Photolab.sln`, um die Projektmappe in Visual Studio zu öffnen.

## <a name="part-1-create-a-fancy-slider-control"></a>Teil 1: Erstellen eines schicken Schiebereglers

Die Windows-App bietet eine Reihe von Möglichkeiten, mit denen du die Darstellung deiner App anpassen kannst – von Schriftarten über Typografieeinstellungen bis hin zu Farben, Farbverläufen und Weichzeichnereffekten.

Im ersten Teil des Tutorials peppen wir einige unserer Steuerelemente für die Fotobearbeitung auf.

![Ein normaler Schieberegler im Standardstil.](../basics/images/xaml-basics/slider-start.png)

Diese Schieberegler sind zwar ganz in Ordnung und tun das, was man von einem Schieberegler erwartet, aber sie sind nichts Besonderes. Das ändern wir gleich.

Der Schieberegler für die Belichtung passt die Belichtung des Bilds an. Wenn du ihn nach links ziehst, wird das Bild dunkler. Ziehst du ihn nach rechts, wird das Bild heller. Wir hübschen unseren Schieberegler nun etwas auf und versehen ihn mit einem schwarzweißen Farbverlauf als Hintergrund. Der Schieberegler sieht dadurch nicht nur besser aus, sondern veranschaulicht auch, welche Funktion er erfüllt.

Drücke F5, um die App zu kompilieren und auszuführen. Der erste Bildschirm zeigt eine Bildergalerie. Klicke auf ein Bild, um die Detailseite aufzurufen. Klicke dort auf die Bearbeitungsschaltfläche, um die Bearbeitungssteuerelemente anzuzeigen, mit denen wir arbeiten. Beende die App, und kehre zu Visual Studio zurück.

### <a name="customize-a-slider-control"></a>Anpassen eines Schiebereglers

1. Doppelklicke im Projektmappen-Explorer auf die Datei DetailPage.xaml, um sie zu öffnen.

    ![Die Datei „DetailPage.xaml“ im Projektmappen-Explorer von Visual Studio.](../basics/images/xaml-basics/style-detail-page-explorer.png)

1. Verwenden Sie ein `Polygon`-Element, um eine Hintergrundform für den Belichtungsschieberegler zu erstellen.

    Im [Namespace „Windows.UI.Xaml.Shapes“](/uwp/api/Windows.UI.Xaml.Shapes) stehen sieben Formen zur Auswahl. Es gibt eine Ellipse, ein Rechteck und einen sogenannten Pfad, mit dem du eine beliebige Form zeichnen kannst (ja, sogar ein Einhorn).

    ![Ein Einhorn](../basics/images/xaml-basics/unicorn.png)

    > **Weitere Informationen:** Im Artikel [Zeichnen von Formen](../controls-and-patterns/shapes.md) erfährst du alles, was du über XAML-Formen wissen musst.

    Wir möchten ein dreieckiges Widget erstellen, das in etwa so aussieht wie die Lautstärkeregelung an einer Stereoanlage.

    ![Ein Lautstärkeregler](../basics/images/xaml-basics/style-volume-slider.png)

    Das hört sich wie eine Aufgabe für die `Polygon`-Form an! Zum Definieren eines Polygons wird eine Gruppe von Punkten angegeben und mit einer Füllung versehen. Unser Polygon soll etwa 200 Pixel breit und 20 Pixel hoch sein und mit einem Farbverlauf gefüllt werden.

    Suche in „DetailPage.xaml“ den Code für den Belichtungsschieberegler, und erstelle direkt davor ein Polygonelement:

    * Lege **Grid.Row** auf „2“ fest, damit sich das Polygon in der gleichen Zeile befindet wie der Belichtungsschieberegler.
    * Lege die Eigenschaft **Points** auf „0,20 200,20 200,0“ fest, um die Dreiecksform zu definieren.
    * Lege die Eigenschaft **Stretch** auf „Fill“ und die Eigenschaft **HorizontalAlignment** auf „Stretch“ fest.
    * Lege **Height** auf „20“ und **VerticalAlignment** auf „Center“ fest.
    * Gib dem **Polygon** einen linearen Farbverlauf.
    * Lege die Eigenschaft **Foreground** für den Belichtungsschieberegler auf „Transparent“ fest, um das Polygon sehen zu können.

    **Vorher**

    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    **Nachher**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
        Grid.Row="2"
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    Anmerkungen:
    * Wie Sie anhand des umgebenden XAML-Codes sehen, befinden sich diese Elemente in einem `Grid`. Wir haben das Polygon in der gleichen Zeile platziert wie den Belichtungsschieberegler (`Grid.Row="2"`), sodass sie an derselben Stelle angezeigt werden. Wir haben das Polygon vor dem Schieberegler platziert, damit der Schieberegler über der Form gerendert wird.
    * Für das Polygon haben wir `Stretch="Fill"` und `HorizontalAlignment="Stretch"` festgelegt, damit das Dreieck so angepasst wird, dass es den verfügbaren Platz ausfüllt. Wenn sich die Breite des Schiebereglers ändert, wird das Polygon entsprechend vergrößert oder verkleinert.

1. Kompiliere die App, und führe sie aus. Der Schieberegler sollte nun wie folgt aussehen:

    ![Ein schicker Belichtungsschieberegler](../basics/images/xaml-basics/style-exposure-slider-done.png)

1. Kommen wir zum nächsten Schieberegler: dem Schieberegler für die Farbtemperatur. Der Temperaturschieberegler ändert die Farbtemperatur des Bilds. Wenn du ihn nach links ziehst, wird das Bild blauer. Ziehst du ihn nach rechts, wird das Bild gelber.

    Für diese Hintergrundform verwenden wir ein weiteres Polygon mit den gleichen Dimensionen wie zuvor, diesmal aber mit einem blau-gelben Farbverlauf anstelle von Schwarzweiß.

    **Vorher**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **Nachher**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

1. Kompiliere die App, und führe sie aus. Du hast nun zwei schicke Schieberegler:

    ![Zwei schicke Schieberegler](../basics/images/xaml-basics/style-2sliders-done.png)

1. **Bonus**

    Füge für den Farbtonschieberegler eine Hintergrundform mit einem rot-grünen Farbverlauf hinzu.

    ![Drei schicke Schieberegler](../basics/images/xaml-basics/style-3sliders-done.png)

Herzlichen Glückwunsch! Der erste Teil ist geschafft. Falls Sie an einer Stelle nicht weiterkommen oder Sie die endgültige Lösung ansehen möchten, finden Sie den vollständigen Code unter [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).

## <a name="part-2-create-basic-styles"></a>Teil 2: Erstellen grundlegender Stile

Einer der Vorteile von XAML-Stilen besteht darin, dass sie den Programmieraufwand erheblich verringern und die Aktualisierung der App-Darstellung deutlich vereinfachen.

Um einen Stil zu definieren, wird der Eigenschaft [Resources](/uwp/api/windows.ui.xaml.frameworkelement.Resources) eines Elements, in dem sich das zu gestaltende Steuerelement befindet, ein Element vom Typ [Style](/uwp/api/Windows.UI.Xaml.Style) hinzugefügt.  Wenn du deinen Stil der Eigenschaft `Page.Resources` hinzufügst, stehen deine Stile für die gesamte Seite zur Verfügung. Wenn du deinen Stil der Eigenschaft `Application.Resources` in der Datei „App.xaml“ hinzufügst, steht der Stil für die gesamte App zur Verfügung.

Du kannst benannte Stile und allgemeine Stile erstellen. Ein benannter Stil muss explizit auf bestimmte Steuerelemente angewendet werden. Ein allgemeiner Stil wird auf alle Steuerelemente angewendet, die dem angegebenen Zieltyp (`TargetType`) entsprechen.

In diesem Beispiel besitzt der erste Stil ein Attribut vom Typ `x:Key` und den Zieltyp `Button`. Die Eigenschaft `Style` der ersten Schaltfläche wird auf diesen Schlüssel festgelegt. Der Stil ist somit ein benannter Stil und muss explizit angewendet werden. Der zweite Stil wird automatisch auf die zweite Schaltfläche angewendet, da deren Zieltyp `Button` lautet und der Stil über kein Attribut vom Typ `x:Key` verfügt.

```xaml
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

Als Nächstes fügen wir unserer App einen Stil hinzu. Sieh dir in „DetailsPage.xaml“ die Textblöcke an, die sich neben unseren Schiebereglern für Belichtung, Farbtemperatur und Farbton befinden. Jeder dieser Textblöcke zeigt den Wert eines Schiebereglers an. Im Anschluss findest du den Textblock für den Belichtungsschieberegler. Wie du siehst, sind die Eigenschaften `Margin`, `VerticalAlignment` und `Padding` festgelegt:

```xaml
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```

Wenn du dir die anderen Textblöcke ansiehst, wirst du feststellen, dass dort die gleichen Eigenschaften und die gleichen Werte festgelegt sind. Klingt nach einer guten Gelegenheit für einen Stil.

### <a name="create-a-value-text-block-style"></a>Erstellen eines Stils mit einem Werttextblock

1. Öffne „DetailsPage.xaml“.

2. Suche nach dem Steuerelement `Grid` mit dem Namen **EditControlsGrid**. Es enthält unsere Schieberegler und Textfelder. Beachte, dass das Raster bereits einen Stil für unsere Schieberegler definiert.

    ```xaml
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

3. Erstelle einen Stil für einen Textblock (**TextBlock**), um **Margin** auf „10,8,0,0“, **VerticalAlignment** auf „Center“ und **Padding** auf „0“ festzulegen.

    **Vorher**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

    **Nachher**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
        </Grid.Resources>
    ```

4. Wir machen daraus einen benannten Stil, damit wir angeben können, für welche Steuerelemente vom Typ `TextBlock` er gelten soll. Legen Sie die `x:Key`-Eigenschaft auf „ValueTextBlock“ fest.

    **Vorher**

    ```xaml
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

    **Nachher**

    ```xaml
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

5. Entfernen Sie für jeden `TextBlock` die Eigenschaften `Margin`, `VerticalAlignment` und `Padding`, und legen Sie die `Style`-Eigenschaft auf „{StaticResource ValueTextBlock}“ fest.

    **Vorher**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    **Nachher**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBlock}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Führe diese Änderung für alle sechs TextBlock-Steuerelemente durch, die den Schiebereglern zugeordnet sind.

6. Kompiliere die App, und führe sie aus. Sie sollte nun... immer noch genauso aussehen. Du kannst allerdings stolz darauf sein, effizienten und mühelos verwaltbaren Code geschrieben zu haben.

Damit ist der zweite Teil abgeschlossen.

## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>Teil 3: Erstellen eines schicken Schiebereglers mithilfe einer Steuerelementvorlage

Du erinnerst dich doch bestimmt noch an den ersten Teil, in dem wir eine Form im Hintergrund des Schiebereglers hinzugefügt haben, um ihn etwas aufzupeppen.

Das hat zwar funktioniert, der gleiche Effekt lässt sich jedoch noch besser mit einer Steuerelementvorlage erzielen.

1. Doppelklicke im Projektmappen-Explorer auf DetailPage.xaml.

1. Als Nächstes verwenden wir die standardmäßige Steuerelementvorlage für Schieberegler als Ausgangspunkt. Füge den folgenden XAML-Code dem Element `Page.Resources` hinzu. (Das Element `Page.Resources` befindet sich am Anfang der Seite.)

    Diese App verwendet die Windows UI Library (WinUI), sodass Sie die Standardvorlage aus dem WinUI-GitHub-Repository kopieren können: [Slider_themeresources.xaml](https://github.com/microsoft/microsoft-ui-xaml/blob/master/dev/Slider/Slider_themeresources.xaml).


    ```xaml
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider"
      xmlns:contract6Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract6NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract7Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,7)"
      xmlns:contract7NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,7)">
      <Grid Margin="{TemplateBinding Padding}">
        <Grid.Resources>
            <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                <Setter Property="BorderThickness" Value="0" />
                <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Thumb">
                            <Border
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{ThemeResource SliderThumbCornerRadius}" />
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Grid.Resources>

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Normal" />

                <VisualState x:Name="Pressed">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="Disabled">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="PointerOver">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>
            <VisualStateGroup x:Name="FocusEngagementStates">
                <VisualState x:Name="FocusDisengaged" />
                <VisualState x:Name="FocusEngagedHorizontal">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
                <VisualState x:Name="FocusEngagedVertical">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>

        </VisualStateManager.VisualStateGroups>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <ContentPresenter x:Name="HeaderContentPresenter"
            Grid.Row="0"
            Content="{TemplateBinding Header}"
            ContentTemplate="{TemplateBinding HeaderTemplate}"
            FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
            Foreground="{ThemeResource SliderHeaderForeground}"
            Margin="{ThemeResource SliderTopHeaderMargin}"
            TextWrapping="Wrap"
            Visibility="Collapsed"
            x:DeferLoadStrategy="Lazy"/>
        <Grid x:Name="SliderContainer"
            Grid.Row="1"
            Background="{ThemeResource SliderContainerBackground}"
            Control.IsTemplateFocusTarget="True">
            <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.RowDefinitions>
                    <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
                </Grid.RowDefinitions>

                <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="TopTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Bottom"
                    Margin="0,0,0,4"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="HorizontalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="BottomTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Top"
                    Margin="0,4,0,0"
                    Grid.Row="2"
                    Grid.ColumnSpan="3" />
                <Thumb x:Name="HorizontalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Height="{ThemeResource SliderHorizontalThumbHeight}"
                    Width="{ThemeResource SliderHorizontalThumbWidth}"
                    Grid.Row="0"
                    Grid.RowSpan="3"
                    Grid.Column="1"
                    FocusVisualMargin="-14,-6,-14,-6"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
            <Grid x:Name="VerticalTemplate" MinWidth="{ThemeResource SliderVerticalWidth}" Visibility="Collapsed">

                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="{ThemeResource SliderPreContentMargin}" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="{ThemeResource SliderPostContentMargin}" />
                </Grid.ColumnDefinitions>

                <Rectangle x:Name="VerticalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="VerticalDecreaseRect"
                    Fill="{TemplateBinding Foreground}"
                    Grid.Column="1"
                    Grid.Row="2"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="LeftTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Right"
                    Margin="0,0,4,0"
                    Grid.RowSpan="3" />
                <TickBar x:Name="VerticalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3" />
                <TickBar x:Name="RightTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Left"
                    Margin="4,0,0,0"
                    Grid.Column="2"
                    Grid.RowSpan="3" />
                <Thumb x:Name="VerticalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Width="{ThemeResource SliderVerticalThumbWidth}"
                    Height="{ThemeResource SliderVerticalThumbHeight}"
                    Grid.Row="1"
                    Grid.Column="0"
                    Grid.ColumnSpan="3"
                    FocusVisualMargin="-6,-14,-6,-14"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
        </Grid>
      </Grid>
    </ControlTemplate>
    ```

    Wow, das ist eine ganze Menge XAML-Code! Steuerelementvorlagen sind ein leistungsfähiges Feature, können aber recht komplex sein. Daher empfiehlt es sich in der Regel, mit der Standardvorlage zu beginnen.

1. Suche in der soeben hinzufügten Steuerelementvorlage (`ControlTemplate`) nach dem Rastersteuerelement namens `HorizontalTemplate`. Dieses Raster definiert den Teil der Vorlage, den wir ändern möchten.

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
        </Grid.RowDefinitions>
    ```

1.  Erstelle ein Polygon mit den gleichen Merkmalen wie das Polygon, das du im ersten Teil für den Belichtungsschieberegler erstellt hast. Füge das Polygon nach dem schließenden Tag `Grid.RowDefinitions` hinzu. Lege `Grid.Row` auf „0“, `Grid.RowSpan` auf „3“ und `Grid.ColumnSpan` auf „3“ fest.

    **Vorher**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

    **Nachher**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

1. Entfernen Sie die `Polygon.Fill`-Einstellung. Legen Sie `Fill` auf `"{TemplateBinding Background}"` fest. Dadurch wird die Eigenschaft `Fill` des Polygons festgelegt, wenn die Eigenschaft `Background` des Schiebereglers festgelegt wird.

    **Vorher**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

    **Nachher**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20"
                    Fill="{TemplateBinding Background}">
        </Polygon>
    ```

1. Direkt nach dem Polygon, das zu hinzugefügt hast, befindet sich ein Rechteck namens `HorizontalTrackRect`. Entferne die Einstellung `Fill` des Rechtecks, damit das Rechteck nicht sichtbar ist und unsere Polygonform nicht blockiert. Wir möchten das Rechteck nicht vollständig entfernen, da es von der Steuerelementvorlage auch für interaktionsbezogene visuelle Hinweise (etwa beim Daraufzeigen) verwendet wird.

    **Vorher**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    **Nachher**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Damit ist die Vorlage fertig. Nun müssen wir sie auf unsere Schieberegler anwenden.

1. Aktualisieren wir also unseren Belichtungsschieberegler.

    * Legen Sie die `Template`-Eigenschaft des Schiebereglers auf `"{StaticResource FancySliderControlTemplate}"` fest.
    * Entfernen Sie die `Background="Transparent"`-Einstellung des Schiebereglers.
    * Legen Sie den `Background` des Schiebereglers auf einen linearen Farbverlauf fest, der von Schwarz in Weiß übergeht.
    * Entferne das Hintergrundpolygon, das wir im ersten Teil erstellt haben.

    **Vorher**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2" />
    ```

    **Nachher**

    ```xaml
    <Slider Header="Exposure"
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. Aktualisiere den Schieberegler für die Farbtemperatur auf die gleiche Weise.

    **Vorher**

    ```xaml
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **Nachher**

    ```xaml
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. Aktualisiere den Farbtonschieberegler auf die gleiche Weise.

    **Vorher**

    ```xaml
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **Nachher**

    ```xaml
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. Kompiliere die App, und führe sie aus.

    ![Die besten Schieberegler überhaupt](../basics/images/xaml-basics/style-sliders-templates.png)

    Wie du siehst, ist das Polygon dank unserer Aktualisierungen nun besser positioniert: Der untere Rand des Polygons ist nun am unteren Rand des Schieberegler-Ziehpunkts ausgerichtet.

Damit ist das Tutorial beendet. Falls Sie nicht weiterkommen und die endgültige Lösung ansehen möchten, finden Sie das vollständige Beispiel unter [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).