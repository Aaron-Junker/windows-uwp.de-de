---
title: Erstellen benutzerdefinierter Stile
description: In diesem Artikel werden die Grundlagen der Gestaltung von UI-Elementen in XAML behandelt.
keywords: XAML, UWP, Erste Schritte
ms.date: 08/31/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d540b41620110a41676d08f5e6239efd0ef4ca46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66361230"
---
# <a name="tutorial-create-custom-styles"></a>Tutorial: Erstellen benutzerdefinierter Stile

In diesem Tutorial wird die Anpassung der Benutzeroberfläche (User Interface, UI) unserer XAML-App veranschaulicht. Warnung: in diesem Tutorial taucht möglicherweise ein Einhorn auf. (Im Ernst!)  

## <a name="prerequisites"></a>Voraussetzungen
* [Visual Studio 2017 und das Windows 10 SDK (10.0.15063.468 oder höher)](https://developer.microsoft.com/windows/downloads)

## <a name="part-0-get-the-code"></a>Teil 0: Beziehen des Codes
Der Ausgangspunkt für dieses Lab befindet sich im PhotoLab-Beispielrepository im Ordner [xaml-basics-starting-points/style/](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/style). Nachdem du das Repository geklont/heruntergeladen hast, kannst du das Projekt bearbeiten, indem du „PhotoLab.sln“ mit Visual Studio 2017 öffnest.

Die PhotoLab-App besteht aus zwei Hauptseiten:

**MainPage.xaml**: Zeigt eine Fotogalerie sowie einige Informationen zur jeweiligen Bilddatei.
![MainPage](../basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml** zeigt ein einzelnes Foto, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.
![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="part-1-create-a-fancy-slider-control"></a>Teil 1: Erstellen eines schicken Schiebereglers  

Die universelle Windows-Plattform (UWP) bietet eine Reihe von Möglichkeiten, mit denen du die Darstellung deiner App anpassen kannst – von Schriftarten über Typografieeinstellungen bis hin zu Farben, Farbverläufen und Weichzeichnereffekten. 

Im ersten Teil des Tutorials peppen wir einige unserer Steuerelemente für die Fotobearbeitung auf. 

<figure>
    <img src="../basics/images/xaml-basics/slider-start.png" />
    <figure>*Ein normaler Schieberegler im Standardstil.*</figure>
</figure>

Diese Schieberegler sind zwar ganz in Ordnung und tun das, was man von einem Schieberegler erwartet, aber sie sind nichts Besonderes. Das ändern wir gleich. 

Der Schieberegler für die Belichtung passt die Belichtung des Bilds an. Wenn du ihn nach links ziehst, wird das Bild dunkler. Ziehst du ihn nach rechts, wird das Bild heller. Wir hübschen unseren Schieberegler nun etwas auf und versehen ihn mit einem schwarzweißen Farbverlauf als Hintergrund. Der Schieberegler sieht dadurch nicht nur besser aus, sondern veranschaulicht auch, welche Funktion er erfüllt.

### <a name="customize-a-slider-control"></a>Anpassen eines Schiebereglers

<!-- TODO: Update folder -->
1. Öffne nach dem Herunterladen des Repositorys die Datei **PhotoLab.sln** aus dem Ordner „xaml-basics-starting-points/style/“, und lege deine Projektmappenplattform auf „x86“ oder „x64“ fest (nicht auf „ARM“). 

    Drücke F5, um die App zu kompilieren und auszuführen. Der erste Bildschirm zeigt eine Bildergalerie. Klicke auf ein Bild, um die Detailseite aufzurufen. Klicke dort auf die Bearbeitungsschaltfläche, um die Bearbeitungssteuerelemente anzuzeigen, mit denen wir arbeiten. Beende die App, und kehre zu Visual Studio zurück.  

2. Doppelklicke im Projektmappen-Explorer auf die Datei **DetailPage.xaml**, um sie zu öffnen. 

    ![Die Datei „DetailPage.xaml“ im Projektmappen-Explorer von Visual Studio 2017.](../basics/images/xaml-basics/style-detail-page-explorer.png)

3. Verwende ein Polygonelement, um eine Hintergrundform für den Belichtungsschieberegler zu erstellen.

    Im [Namespace „Windows.XAML.Ui.Shapes“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Shapes) stehen sieben Formen zur Auswahl. Es gibt eine Ellipse, ein Rechteck und einen sogenannten Pfad, mit dem du eine beliebige Form zeichnen kannst (ja, sogar ein Einhorn). 
    
    <!-- TODO reduce size -->
    ![Ein Einhorn](../basics/images/xaml-basics/unicorn.png)
    
    > **Weitere Informationen:** Im Artikel [Zeichnen von Formen](https://docs.microsoft.com/en-us/windows/uwp/graphics/drawing-shapes) erfährst du alles, was du über XAML-Formen wissen musst. 
    
    Wir möchten ein dreieckiges Widget erstellen, das in etwa so aussieht wie die Lautstärkeregelung an einer Stereoanlage.
    
    ![Ein Lautstärkeregler](../basics/images/xaml-basics/style-volume-slider.png)
    
    Hierzu verwenden wir am besten die Polygonform. Zum Definieren eines Polygons wird eine Gruppe von Punkten angegeben und mit einer Füllung versehen. Unser Polygon soll etwa 200 Pixel breit und 20 Pixel hoch sein und mit einem Farbverlauf gefüllt werden.
    
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

    Hinweise:
    * Wie du anhand des umgebenden XAML-Codes siehst, befinden sich diese Elemente in einem Raster. Wir haben das Polygon in der gleichen Zeile platziert wie den Belichtungsschieberegler (Grid.Row="2"), sodass sie an derselben Stelle angezeigt werden. Wir haben das Polygon vor dem Schieberegler platziert, damit der Schieberegler über der Form gerendert wird.
    * Für das Polygon haben wir „Stretch” auf „Fill“ und „HorizontalAlignment” auf „Stretch“ festgelegt, damit das Dreieck so angepasst wird, dass es den verfügbaren Platz ausfüllt. Wenn sich die Breite des Schiebereglers ändert, wird das Polygon entsprechend vergrößert oder verkleinert. 

4. Kompiliere die App, und führe sie aus. Der Schieberegler sollte nun wie folgt aussehen:

    ![Ein schicker Belichtungsschieberegler](../basics/images/xaml-basics/style-exposure-slider-done.png)

5. Kommen wir zum nächsten Schieberegler: dem Schieberegler für die Farbtemperatur. Der Temperaturschieberegler ändert die Farbtemperatur des Bilds. Wenn du ihn nach links ziehst, wird das Bild blauer. Ziehst du ihn nach rechts, wird das Bild gelber.

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

6. Kompiliere die App, und führe sie aus. Du hast nun zwei schicke Schieberegler:

    ![Zwei schicke Schieberegler](../basics/images/xaml-basics/style-2sliders-done.png)

7. **Bonus**

    Füge für den Farbtonschieberegler eine Hintergrundform mit einem rot-grünen Farbverlauf hinzu. 

    ![Drei schicke Schieberegler](../basics/images/xaml-basics/style-3sliders-done.png)


Herzlichen Glückwunsch! Der erste Teil ist geschafft. Falls du an einer Stelle nicht weiterkommst oder dir die endgültige Lösung ansehen möchtest, findest du den fertigen Code unter **UWP Academy\XAML\Styling\Part1\Finish**.

 
    
## <a name="part-2-create-basic-styles"></a>Teil 2: Erstellen grundlegender Stile

Einer der Vorteile von XAML-Stilen besteht darin, dass sie den Programmieraufwand erheblich verringern und die Aktualisierung der App-Darstellung deutlich vereinfachen.

Um einen Stil zu definieren, wird der Eigenschaft [Resources](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.Resources) eines Elements, in dem sich das zu gestaltende Steuerelement befindet, ein Element vom Typ [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) hinzugefügt.  Wenn du deinen Stil der Eigenschaft **Page.Resources** hinzufügst, stehen deine Stile für die gesamte Seite zur Verfügung. Wenn du deinen Stil der Eigenschaft **Application.Resources** in der Datei „App.xaml“ hinzufügst, steht der Stil für die gesamte App zur Verfügung.

Du kannst benannte Stile und allgemeine Stile erstellen. Ein benannter Stil muss explizit auf bestimmte Steuerelemente angewendet werden. Ein allgemeiner Stil wird auf alle Steuerelemente angewendet, die dem angegebenen Zieltyp (**TargetType**) entsprechen. 

In diesem Beispiel besitzt der erste Stil ein Attribut vom Typ **x:Key** und den Zieltyp **Button**. Die Eigenschaft **Style** der ersten Schaltfläche wird auf diesen Schlüssel festgelegt. Der Stil ist somit ein benannter Stil und muss explizit angewendet werden. Der zweite Stil wird automatisch auf die zweite Schaltfläche angewendet, da deren Zieltyp **Button** lautet und der Stil über kein Attribut vom Typ **x:Key** verfügt.


```XAML
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

Als Nächstes fügen wir unserer App einen Stil hinzu. Sieh dir in „DetailsPage.xaml“ die Textblöcke an, die sich neben unseren Schiebereglern für Belichtung, Farbtemperatur und Farbton befinden. Jeder dieser Textblöcke zeigt den Wert eines Schiebereglers an. Im Anschluss findest du den Textblock für den Belichtungsschieberegler. Wie du siehst, sind die Eigenschaften **Margin**, **VerticalAlignment** und **Padding** festgelegt:

```XAML
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```
Wenn du dir die anderen Textblöcke ansiehst, wirst du feststellen, dass dort die gleichen Eigenschaften und die gleichen Werte festgelegt sind. Klingt nach einer guten Gelegenheit für einen Stil.

### <a name="create-a-value-text-block-style"></a>Erstellen eines Stils mit einem Werttextblock

<!-- TODO: add second starting point -->
1. Öffne „DetailsPage.xaml“.

2. Suche nach dem Steuerelement **Grid** mit dem Namen **EditControlsGrid**. Es enthält unsere Schieberegler und Textfelder. Beachte, dass das Raster bereits einen Stil für unsere Schieberegler definiert. 

    ```XAML
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
    ```XAML
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
    ```XAML
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

4. Wir machen daraus einen benannten Stil, damit wir angeben können, für welche Steuerelemente vom Typ **TextBlock** er gelten soll. Lege die Eigenschaft **x: Key** des Stils auf „ValueTextBox“ fest. 

    **Vorher**
    ```XAML
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
    ```XAML
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBox">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
    ```    

5. Entferne für jeden Textblock (**TextBlock**) die Eigenschaften **Margin**, **VerticalAlignment** und **Padding**, und lege die Eigenschaft **Style** auf „{StaticResource ValueTextBox}“ fest.

    **Vorher**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```

    **Nachher**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBox}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```    

    Führe diese Änderung für alle sechs TextBlock-Steuerelemente durch, die den Schiebereglern zugeordnet sind.

6. Kompiliere die App, und führe sie aus. Sie sollte nun... immer noch genauso aussehen. Du kannst allerdings stolz darauf sein, effizienten und mühelos verwaltbaren Code geschrieben zu haben.

<!-- TODO add new start/end points -->
Damit ist der zweite Teil abgeschlossen.


## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>Teil 3: Erstellen eines schicken Schiebereglers mithilfe einer Steuerelementvorlage

Du erinnerst dich doch bestimmt noch an den ersten Teil, in dem wir eine Form im Hintergrund des Schiebereglers hinzugefügt haben, um ihn etwas aufzupeppen.

Das hat zwar funktioniert, der gleiche Effekt lässt sich jedoch noch besser mit einer Steuerelementvorlage erzielen. 

<!-- TODO add new starting points -->
1. Doppelklicke im Projektmappen-Explorer auf **DetailPage.xaml**.

2. Als Nächstes verwenden wir die standardmäßige Steuerelementvorlage für Schieberegler als Ausgangspunkt. Füge den folgenden XAML-Code dem Element **Page.Resources** hinzu. (Das Element **Page.Resources** befindet sich am Anfang der Seite.)

    ```XAML
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider">
        <Grid Margin="{TemplateBinding Padding}">
            <Grid.Resources>
                <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                    <Setter Property="BorderThickness" Value="0" />
                    <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                    <Setter Property="Template">
                        <Setter.Value>
                            <ControlTemplate TargetType="Thumb">
                                <Border Background="{TemplateBinding Background}"
                                            BorderBrush="{TemplateBinding BorderBrush}"
                                            BorderThickness="{TemplateBinding BorderThickness}"
                                            CornerRadius="4" />
                            </ControlTemplate>
                        </Setter.Value>
                    </Setter>
                </Style>
            </Grid.Resources>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
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
            <ContentPresenter x:Name="HeaderContentPresenter"
                        x:DeferLoadStrategy="Lazy"
                        Visibility="Collapsed"
                        Foreground="{ThemeResource SliderHeaderForeground}"
                        Margin="{ThemeResource SliderHeaderThemeMargin}"
                        Content="{TemplateBinding Header}"
                        ContentTemplate="{TemplateBinding HeaderTemplate}"
                        FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
                        TextWrapping="Wrap" />
            <Grid x:Name="SliderContainer"
                        Background="{ThemeResource SliderContainerBackground}"
                        Grid.Row="1"
                        Control.IsTemplateFocusTarget="True">
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
                    <Rectangle x:Name="HorizontalTrackRect"
                                Fill="{TemplateBinding Background}"
                                Height="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Row="1"
                                Grid.ColumnSpan="3" />
                    <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1" />
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
                                Height="24"
                                Width="8"
                                Grid.Row="0"
                                Grid.RowSpan="3"
                                Grid.Column="1"
                                FocusVisualMargin="-14,-6,-14,-6"
                                AutomationProperties.AccessibilityView="Raw" />
                </Grid>
                <Grid x:Name="VerticalTemplate" MinWidth="44" Visibility="Collapsed">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="*" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="18" />
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="18" />
                    </Grid.ColumnDefinitions>
                    <Rectangle x:Name="VerticalTrackRect"
                                Fill="{TemplateBinding Background}"
                                Width="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Column="1"
                                Grid.RowSpan="3" />
                    <Rectangle x:Name="VerticalDecreaseRect"
                                Fill="{TemplateBinding Foreground}"
                                Grid.Column="1"
                                Grid.Row="2" />
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
                                Width="24"
                                Height="8"
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
    
3. Suche in der soeben hinzufügten Steuerelementvorlage (**ControlTemplate**) nach dem Rastersteuerelement namens **HorizontalTemplate**. Dieses Raster definiert den Teil der Vorlage, den wir ändern möchten.

    ```XAML
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

5.  Erstelle ein Polygon mit den gleichen Merkmalen wie das Polygon, das du im ersten Teil für den Belichtungsschieberegler erstellt hast. Füge das Polygon nach dem schließenden Tag **Grid.RowDefinitions** hinzu. Lege **Grid.Row** auf „0“, **Grid.RowSpan** auf „3“ und **Grid.ColumnSpan** auf „3“ fest. 

    **Vorher**
    ```XAML
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
    ```XAML
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

6. Entferne die Einstellung **Polygon.Fill**. Lege **Fill** auf „{TemplateBinding Background}“ fest. Dadurch wird die Eigenschaft **Fill** des Polygons festgelegt, wenn die Eigenschaft **Background** des Schiebereglers festgelegt wird. 

    **Vorher**
    ```XAML
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
    ```XAML
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" 
                    Fill="{TemplateBinding Background}">
        </Polygon>           
    ```    

7. Direkt nach dem Polygon, das zu hinzugefügt hast, befindet sich ein Rechteck namens **HorizontalTrackRect**. Entferne die Einstellung **Fill** des Rechtecks, damit das Rechteck nicht sichtbar ist und unsere Polygonform nicht blockiert. Wir möchten das Rechteck nicht vollständig entfernen, da es von der Steuerelementvorlage auch für interaktionsbezogene visuelle Hinweise (etwa beim Daraufzeigen) verwendet wird.

    **Vorher**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />          
    ```
    
    **Nachher**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Damit ist die Vorlage fertig. Nun müssen wir sie auf unsere Schieberegler anwenden. 
    
8. Aktualisieren wir also unseren Belichtungsschieberegler.

    * Lege die Eigenschaft **template** des Schiebereglers auf „{StaticResource FancySliderControlTemplate}“ fest.
    * Entferne die Einstellung „Background="Transparent"“ des Schiebereglers. 
    * Lege den Hintergrund des Schiebereglers auf einen linearen Farbverlauf fest, der von Schwarz in Weiß übergeht.
    * Entferne das Hintergrundpolygon, das wir im ersten Teil erstellt haben.
        
    **Vorher**
    ```XAML
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
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}"/>    
    ```
    
    **Nachher**
    ```XAML
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
9. Aktualisiere den Schieberegler für die Farbtemperatur auf die gleiche Weise.

    **Vorher**
    ```XAML
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
    ```XAML
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

10. Aktualisiere den Farbtonschieberegler auf die gleiche Weise.

    **Vorher**
    ```XAML
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
    ```XAML
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

11. Kompiliere die App, und führe sie aus. 

    ![Die besten Schieberegler überhaupt](../basics/images/xaml-basics/style-sliders-templates.png)
    
    Wie du siehst, ist das Polygon dank unserer Aktualisierungen nun besser positioniert: Der untere Rand des Polygons ist nun am unteren Rand des Schieberegler-Ziehpunkts ausgerichtet.
    
<!-- TODO correct folder -->
Damit ist das Tutorial beendet. Falls du an einer Stelle nicht weiterkommen solltest und dir die endgültige Lösung ansehen möchtest, findest du das vollständige Beispiel im [UWP-App-Beispielrepository](https://github.com/Microsoft/Windows-universal-samples).