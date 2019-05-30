---
title: Erstellen eigener Stile
description: In diesem Artikel werden die Grundlagen für UI-Stilelemente in XAML behandelt
keywords: XAML, UWP, Erste Schritte
ms.date: 08/31/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d540b41620110a41676d08f5e6239efd0ef4ca46
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361230"
---
# <a name="tutorial-create-custom-styles"></a>Tutorial: Erstellen eigener Stile

In diesem Lernprogramm wird das Anpassen der Benutzeroberfläche unserer XAML-App veranschaulicht. Warnung: in diesem Lernprogramm kann möglicherweise ein Einhorn auftauchen. (Stimmt!)  

## <a name="prerequisites"></a>Vorraussetzungen
* [Visual Studio 2017 und das Windows 10 SDK (10.0.15063.468 oder höher)](https://developer.microsoft.com/windows/downloads)

## <a name="part-0-get-the-code"></a>Teil 0: Code abrufen
Der Ausgangspunkt für diese Übung befindet sich im PhotoLab-Beispielrepository, unter [xaml-basics-starting-points/style/ Ordner](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/style). Nachdem Sie das Repository geklont/heruntergeladen haben, können Sie das Projekt bearbeiten, indem Sie PhotoLab.sln mit Visual Studio 2017 öffnen.

Die PhotoLab-App besteht aus zwei Hauptseiten:

**MainPage.xaml:** zeigt eine Ansicht der Foto-Galerie, zusammen mit einigen Informationen über jede Bilddatei an.
![MainPage](../basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.
![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="part-1-create-a-fancy-slider-control"></a>Teil 1: Erstellen Sie ein ausgefallener Schieberegler-Steuerelement  

Die universelle Windows-Plattform (UWP) bietet eine Reihe von Möglichkeiten zum Anpassen der Darstellung Ihrer App. Von Schriftarten und Typografie-Einstellungen bis hin zu Farben und Farbverläufen und Weichzeichnereffekten – Sie haben viele Optionen. 

Im ersten Teil des Lernprogramms lernen wir über unsere Steuerelemente zum Bearbeiten von Fotos. 

<figure>
    <img src="../basics/images/xaml-basics/slider-start.png" />
    <figure>*Einen humble Schieberegler mit einer Standardstil.*</figure>
</figure>

Diese Schieberegler sind in Ordnung – sie funktionieren so, wie ein Schieberegler funktionieren sollte – aber sie haben keine ausgefallenen Effekte. Das wird hier geändert. 

Der Schieberegler für die Belichtung passt die Belichtung des Bilds an: ziehen Sie es auf die linke Seite und das Bild wird dunkler; Schieberegler nach rechts, und es wird heller. Machen wir unseren Schieberegler etwas cooler und verleihen wir ihm einen Hintergrund, der von Schwarz auf Weiß verläuft. Der Schieberegler sieht besser aus, was großartige ist, aber es gibt auch einen visuellen Hinweis zu den Funktionen an, die der Schieberegler bereitstellt.

### <a name="customize-a-slider-control"></a>Anpassen des Schieberegler-Steuerelements

<!-- TODO: Update folder -->
1. Öffnen Sie nach dem Download des Repositorys **PhotoLab.sln** unter xaml-basics-starting-points/style/ Ordner, und legen Sie Ihre Projektmappen-Plattform auf x86 oder x64 (nicht ARM) fest. 

    Drücken Sie F5, um die App zu übersetzen und auszuführen. Der erste Bildschirm zeigt eine Bildergalerie. Klicken Sie auf ein Bild, um die Detailseite aufzurufen. Klicken Sie von dort auf die Schaltfläche "Bearbeiten", um die Bearbeitungssteuerelemente zu sehen, mit denen wir arbeiten werden. Beenden Sie die App und kehren Sie zu Visual Studio zurück.  

2. Doppelklicken Sie im Projektmappen-Explorer-Bereich auf **DetailPage.xaml**, um sie zu öffnen. 

    ![Die DetailPage.xaml-Datei im Projektmappen-Explorer von Visual Studio 2017.](../basics/images/xaml-basics/style-detail-page-explorer.png)

3. Verwenden Sie ein Polygon-Element, um eine Hintergrundform für den Schieberegler für die Belichtung zu erstellen.

    Der [Windows.XAML.Ui.Shapes Namespace](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Shapes) enthält sieben Formen zur Auswahl. Es gibt eine Ellipse, ein Rechteck, und einen „Pfad”, der jede Form zeichnen kann – sogar ein Einhorn! 
    
    <!-- TODO reduce size -->
    ![Eines Einhorns](../basics/images/xaml-basics/unicorn.png)
    
    > **Weitere Informationen finden sie ein:** Die [Zeichnen von Formen](https://docs.microsoft.com/en-us/windows/uwp/graphics/drawing-shapes) Artikel erfahren Sie alles, was Sie XAML-Formen kennen müssen. 
    
    Wir möchten ein dreieckig aussehendes Grafikobjekt erstellen – ähnlich der Form der Lautstärkeregelung einer Stereoanlage.
    
    ![Ein Schieberegler](../basics/images/xaml-basics/style-volume-slider.png)
    
    Klingt wie einen Auftrag für die Polygonform! Um einen Polygon zu definieren, geben Sie eine Reihe von Punkten an und weisen Sie ihnen eine Fläche zu. Lassen Sie uns ein Polygon erstellen, das ungefähr 200 Pixel breit und 20 Pixel hoch ist, das mit dem Farbverlauf gefüllt werden soll.
    
    Suchen Sie in DetailPage.xaml den Code für den Schieberegler für die Belichtung heraus und erstellen Sie ein Polygon-Element direkt davor: 

    * Legen Sie **Grid.Row** auf "2" fest, um das Polygon in der gleichen Zeile wie den Belichtungsschieberegler zu platzieren. 
    * Legen Sie die **Punkt**-Eigenschaft auf "0,20 200,20 200,0" fest, um die Dreiecksform zu definieren.
    * Legen Sie die **Stretch**-Eigenschaft auf "Fill" und die **HorizontalAlignment**-Eigenschaft auf "Stretch" fest.
    * Legen Sie die **Höhe** auf "20" und das **VerticalAlignment** auf "Center"fest. 
    * Geben Sie dem **Polygon** einen linearen Farbverlauf.     
    * Legen Sie für den Belichtungsschieberegler die **Vordergrund**-Eigenschaft auf "Transparent" fest, damit Sie das Polygon sehen können. 

    **Vor dem**
    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```
    **After**
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
    * Wenn Sie die umgebende XAML-Codierung betrachten, sehen Sie, dass diese Elemente in einem Raster sind. Wir haben das Polygon in die gleichen Zeile wie den Belichtungsschieberegler (Grid.Row="2") gesetzt, damit sie an derselben Stelle angezeigt werden. Wir haben das Polygon vor den Schieberegler gesetzt, damit der Schieberegler über der Form rendert.
    * Wir haben „Stretch” = "Fill" und „HorizontalAlignment” = "Stretch" im Polygon festgelegt, damit das Dreieck so angepasst wird, um den verfügbaren Platz auszufüllen. Wenn Sie den Schieberegler in der Breite verkleinern oder vergrößern wird das Polygon entsprechend vergrößert oder verkleinert. 

4. Übersetzen Sie die App, und führen Sie sie aus. Der Schieberegler sollte nun großartig aussehen:

    ![Ein ausgefallener Belichtungsschieberegler](../basics/images/xaml-basics/style-exposure-slider-done.png)

5. Geben wir jetzt dem nächsten Schieberegler, dem Schieberegler für die Temperatur, ein Upgrade. Der Temperatur-Schieberegler ändert die Farbtemperatur des Bilds. Beim Ziehen auf die linken Seite wird das Bild blauer und beim Ziehen auf die recht Seite wird das Bild mehr gelb.

    Wir verwenden einen anderen Polygon für diese Hintergrundform mit denselben Dimensionen wie das vorherige, aber dieses Mal ist die Füllung ein Farbverlauf in Blau-Gelb anstelle von Schwarzweiß. 

    **Vor dem**
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
    **After**
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

6. Übersetzen Sie die App, und führen Sie sie aus. Sie verfügen jetzt über ausgefallene Schieberegler.

    ![Zwei ausgefallene Schieberegler](../basics/images/xaml-basics/style-2sliders-done.png)

7. **Bonus**

    Fügen Sie eine Hintergrundform für den Farbton-Schieberegler hinzu, mit einem Farbverlauf von Grün auf Rot. 

    ![Drei ausgefallene Schieberegler](../basics/images/xaml-basics/style-3sliders-done.png)


Herzlichen Glückwunsch! Sie haben Teil 1 abgeschlossen! Wenn Sie nicht weiterkommen oder die endgültige Lösung nicht anzeigen können, finden Sie den fertigen Code unter **UWP Academy\XAML\Styling\Part1\Finish**.

 
    
## <a name="part-2-create-basic-styles"></a>Teil 2: Grundlegende Formate erstellen

Einer der Vorteile der XAML-Stile ist, dass sie die Menge an Code, den Sie schreiben müssen erheblich verringern können und es jetzt viel einfach ist, das Erscheinungsbild Ihrer App zu aktualisieren.

Um einen Stil zu definieren, fügen Sie ein [Stil](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)-Element in die [Ressourcen](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.Resources)-Eigenschaft eines Elements ein, das das Steuerelement enthält, das Sie formatieren möchten.  Wenn Sie Ihren Stil der **Page.Resources**-Eigenschaft hinzufügen, werden Ihre Stile für die gesamte Seite zugängig. Wenn Sie Ihren Stil der **Application.Resources**-Eigenschaft in der Datei App.xaml hinzufügen, wird der Stil wird für die gesamte App zugänglich.

Sie können benannte Stile und allgemeine Stile erstellen. Eine benannte Formatvorlage muss explizit auf bestimmte Steuerelemente angewendet werden. Ein allgemeiner Stil gilt für alle Steuerelemente, die dem angegebenen **TargetType** entsprechen. 

In diesem Beispiel besitzt der erste Stil ein **x:Key-Attribut**, und der Zieltyp ist **Button**. Die **Style**-Eigenschaft der ersten Schaltfläche wird auf diesen Schlüssel festgelegt, sodass der Stil benannt wird und explizit angewendet wird. Der zweite Stil wird automatisch auf die zweite Schaltfläche angewendet, da deren Zieltyp **Button** ist und der Stil kein **x:Key**-Attribut besitzt.


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

Fügen wir nun der App einen Stil hinzu. Sehen Sie sich unter DetailsPage.xaml die Textblöcke an, die sich neben den Belichtungs-, Temperatur- und Farbton-Schiebereglern befinden. Jeder dieser Text-Blöcke zeigt den Wert eines Schiebereglers an. Hier ist der Textblock für den Belichtungs-Schieberegler. Sie sehen, dass die Eigenschaften für **Margin**, **VerticalAlignment** und **Padding** festgelegt sind.

```XAML
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```
Sehen Sie sich die anderen Textblöcke an – Sie sehen, dass die gleichen Eigenschaften auf den gleichen Wert festgelegt sind. Klingt gut für einen Stil...

### <a name="create-a-value-text-block-style"></a>Erstellen eines Wert-Stils für den Text-Block

<!-- TODO: add second starting point -->
1. Öffnen Sie DetailsPage.xaml.

2. Suchen Sie das **Raster**-Steuerelement **EditControlsGrid**. Es enthält unsere Schieberegler und Textfelder. Beachten Sie, dass das Raster bereits ein Format für unsere Schieberegler definiert. 

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
3. Erstellen Sie einen Stil für einen **TextBlock**, der **Margin** auf "10,8,0,0", sein **VerticalAlignment** auf Center" und sein **Padding** auf "0" festlegt.

    **Vor dem**
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

    **After**
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

4. Erstellen wir daraus eine benannte Formatvorlage, damit wir angeben können, welche **TextBlock**-Steuerelemente dafür gelten. Legen Sie die **x: Key** -Eigenschaft des Stils auf "ValueTextBox" fest. 

    **Vor dem**
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

    **After**
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

5. Entfernen Sie für jeden **TextBlock** die Eigenschaften **Margin**, **VerticalAlignment** und **Padding** und legen Sie die **Stil**-Eigenschaft auf "{StaticResource ValueTextBox}" fest.

    **Vor dem**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```

    **After**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBox}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```    

    Nehmen Sie diese Änderung an allen 6 TextBlock-Steuerelementen vor, die den Schiebereglern zugeordnet sind.

6. Übersetzen Sie die App, und führen Sie sie aus. Es sollte... gleich aussehen. Allerdings sollten Sie ein Gefühl der Zufriedenheit haben, das sich daraus ergibt, effizienten und leichter zu verwaltenden Code zu schreiben.

<!-- TODO add new start/end points -->
Herzlichen Glückwunsch! Sie haben Teil 2 abgeschlossen!


## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>Teil 3: Verwenden einer Steuerelementvorlage, um einen Schieberegler mit Effekten zu machen.

Erinnern Sie sich, wie wir in Teil 1 eine Form hinter dem Schieberegler hinzugefügt haben, damit er cool aussieht?

Jetzt gibt es eine bessere Möglichkeit, den gleichen Effekt zu erzielen: Erstellen Sie eine Steuerelementvorlage. 

<!-- TODO add new starting points -->
1. Doppelklicken Sie im Projektmappen-Explorer-Bereich auf **DetailPage.xaml**.

2. Als Nächstes verwenden wir die standardmäßige Steuerelementvorlage für Schieberegler als Ausgangspunkt. Fügen Sie diesen XAML-Code dem **Page.Resources**-Element hinzu. (Das **Page.Resources**-Element befindet sich am Anfang der Seite.)

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

    WOW, das ist eine Menge XAML! Steuerelementvorlagen sind ein leistungsfähiges Feature, sie können jedoch sehr komplex sein, daher empfiehlt sich in der Regel, mit der Standardvorlage zu beginnen. 
    
3. Suchen Sie im gerade hinzugefügten **ControlTemplate** das Raster-Steuerelement mit dem Namen **HorizontalTemplate**. Dieses Raster definiert den Teil der Vorlage, die wir ändern möchten.

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

5.  Erstellen Sie ein Polygon, das genauso funktioniert wie das Polygon, das Sie für den Belichtungs-Schieberegler in Teil 1 erstellt haben. Fügen Sie das Polygon hinzu, nachdem Sie den **Grid.RowDefinitions**-Tag geschlossen haben. Legen Sie **Grid.Row** auf "0", **Grid.RowSpan** auf "3" und **Grid.ColumnSpan** auf "3" fest. 

    **Vor dem**
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

    **After**
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

6. Entfernen Sie die **Polygon.Fill**-Einstellung. Legen Sie **Fill** auf "{TemplateBinding Background}" fest. Dadurch wird die **Hintergrund**-Eigenschaft des Schiebereglers auf die Eigenschaft **Fill** des Polygons festgelegt. 

    **Vor dem**
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
    
    **After**
    ```XAML
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" 
                    Fill="{TemplateBinding Background}">
        </Polygon>           
    ```    

7. Hinter dem Polygon, den Sie gerade hinzugefügt haben, wird ein Rechteck mit dem Namen **HorizontalTrackRect** angezeigt. Entfernen Sie die **Fill**-Eigenschaft des Rechtecks, damit das Rechteck nicht angezeigt wird und unsere Polygonform nicht blockiert. (Wir möchten das Rechteck nicht vollständig entfernen, da die Steuerelementvorlage auch für die Interaktion visueller Objekte wie z. B. Hover verwendet wird.)

    **Vor dem**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />          
    ```
    
    **After**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Herzlichen Glückwunsch! Sie haben diese Vorlage vollständig bearbeitet! Nun müssen wir sie auf unsere Schieberegler anwenden. 
    
8. Aktualisieren wir unseren Belichtungs-Schieberegler.

    * Legen Sie die **Vorlage**-Eigenschaft des Schiebereglers auf "{StaticResource FancySliderControlTemplate}" fest.
    * Entfernen Sie die Einstellung des Schiebereglers für den Hintergrund = "Transparent". 
    * Legen Sie den Schiebereglerhintergrund auf einen linearen Farbverlauf fest, der von Schwarz in Weiß übergeht.
    * Entfernen Sie das Hintergrund-Polygon, das wir in Teil 1 erstellt haben.
        
    **Vor dem**
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
    
    **After**
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
9. Führen Sie die gleichen Updates auf den Schieberegler für die Temperatur aus.

    **Vor dem**
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
    
    **After**
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

10. Führen Sie die gleichen Updates auf den Schieberegler für den Farbton aus.

    **Vor dem**
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
    
    **After**
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

11. Übersetzen Sie die App, und führen Sie sie aus. 

    ![Sie haben die besten Schieberegler in der ganzen Welt](../basics/images/xaml-basics/style-sliders-templates.png)
    
    Wie Sie sehen können, verbessern unsere Updates die Positionierung des Polygons. Jetzt ist das untere Ende des Polygons am unteren Rand der Ziehpunkt des Schiebereglers ausgerichtet.
    
<!-- TODO correct folder -->
Herzlichen Glückwunsch! Sie haben das Lernprogramm abgeschlossen. Wenn Sie nicht weiterkommen oder die endgültige Lösung nicht anzeigen können, finden Sie das fertige Beispiel unter [UWP-App-Beispielrepository](https://github.com/Microsoft/Windows-universal-samples).