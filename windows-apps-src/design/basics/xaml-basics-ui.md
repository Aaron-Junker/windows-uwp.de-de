---
title: Erstellen einer Benutzeroberfläche – Tutorial
description: In diesem Artikel werden die Grundlagen beim Erstellen von Benutzeroberflächen in XAML beschrieben.
keywords: XAML, UWP, Erste Schritte
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1bae8455f1062b3ad62aeac3807c6c58ae274a1b
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63794837"
---
# <a name="tutorial-create-a-user-interface"></a>Tutorial: Erstellen einer Benutzeroberfläche

In diesem Tutorial erfahren Sie, wie Sie eine einfache Benutzeroberfläche für ein Bildbearbeitungsprogramm erstellen: 

+ Unter Verwendung der XAML-Tools in Visual Studio, z. B. XAML-Designer, Toolbox, XAML-Editor, Eigenschaftenbereich und Dokumentgliederung, zum Hinzufügen von Steuerelementen und Inhalten in der Benutzeroberfläche
+ Unter Verwendung einiger der häufigsten XAML-Layoutpanels, z. B. RelativePanel, Grid und StackPanel

Das Bildbearbeitungsprogramm umfasst zwei Seiten/Bildschirme:

Die **Hauptseite** zeigt eine Ansicht der Foto-Galerie, zusammen mit einigen Informationen über jede Bilddatei an.

![MainPage](images/xaml-basics/mainpage.png)

Die **Detailseite** zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio 2017: [Visual Studio 2017 Community herunterladen (kostenlos)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK (10.0.15063.468 oder höher):  [Das aktuelle Windows SDK herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Startcode von Github holen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels. 

1. Wechseln Sie zu [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Sie gelangen auf die GitHub-Seite für das Beispiel. 
2. Als Nächstes müssen Sie das Beispiel klonen oder herunterladen. Klicken Sie auf die Schaltfläche **Clone or download**. Ein Untermenü erscheint.
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Das Menü <b>Clone or download</b> auf der GitHub-Seite des Fotobeispiels.</figcaption>
    </figure>

    **Wenn Sie mit GitHub nicht vertraut sind:**
    
    a. Klicken Sie auf **Download ZIP**, und speichern Sie die Datei lokal. Es wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.
    b. Entpacken Sie die Datei. Verwenden Sie den Datei-Explorer, um zu der gerade heruntergeladenen ZIP-Datei zu navigieren, klicken Sie mit der rechten Maustaste darauf und wählen Sie **Alle extrahieren...** aus. c. Navigieren Sie zu Ihrer lokalen Kopie des Beispiels, und wechseln Sie zum Verzeichnis `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.    

    **Wenn Sie mit GitHub vertraut sind:**

    a. Klonen Sie den Master-Branch des Repositorys lokal.
    b. Wechseln Sie zum `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`-Verzeichnis.

3. Öffnen Sie das Projekt durch einen Klick auf `Photolab.sln`.

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>Teil 1: Hinzufügen eines TextBlock-Steuerelements mit XAML-Designer

Visual Studio bietet verschiedene Tools zum leichteren Erstellen der XAML-UI. Mit XAML-Designer können Sie Steuerelemente auf die Designoberfläche ziehen und vor dem Ausführen der App anschauen. Im Eigenschaftenbereich können Sie alle Eigenschaften des Steuerelements, die im Designer aktiv sind, überprüfen und festlegen. Die Dokumentgliederung zeigt die über- und untergeordnete Struktur der visuellen XAML-Struktur für Ihre Benutzeroberfläche. Mit dem XAML-Editor können Sie das XAML-Markup direkt eingeben und ändern.

Hier ist die Visual Studio-UI mit den bezeichneten Tools.

![Layout in Visual Studio](images/xaml-basics/visual-studio-tools.png)

Diese Tools helfen Ihnen beim einfachen Erstellen der Benutzeroberfläche, deshalb werden sie alle in diesem Lernprogramm verwendet. Beginnen Sie mit XAML-Designer, um ein Steuerelement hinzuzufügen. 

**Hinzufügen eines Steuerelements mithilfe von XAML-Designer:**

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei „MainPage.xaml“, um sie zu öffnen. Dadurch wird die Hauptseite der App angezeigt, ohne dass ein UI-Element hinzugefügt wurde.

2. Vor dem Fortfahren müssen Sie einige Anpassungen in Visual Studio vornehmen.

    - Stellen Sie sicher, dass Ihre Projektmappenplattform auf x86 oder x64 und nicht auf ARM festgelegt ist.
    - Legen Sie auf der Hauptseite des XAML-Designers die Desktopvorschau auf 13,3" fest.

    Beide Einstellungen sollten im oberen Bereich des Fensters angezeigt werden, wie hier veranschaulicht.

    ![VS-Einstellungen](images/xaml-basics/layout-vs-settings.png)

    Sie können die App jetzt ausführen, es wird allerdings nicht viel angezeigt. Lassen Sie uns einige UI-Elemente einfügen, um die Dinge interessanter zu gestalten.

3. Erweitern Sie in der Toolbox **Allgemeine XAML-Steuerelemente** und suchen Sie das [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock)-Steuerelement. Ziehen Sie einen TextBlock auf die Designoberfläche neben die obere linke Ecke der Seite.

    Der TextBlock wird zur Seite hinzugefügt, und der Designer legt einige Eigenschaften auf Grundlage seiner Einschätzung des gewünschten Layouts fest. Der TextBlock wird blau hervorgehoben, womit angezeigt wird, dass es sich dabei um das aktive Objekt handelt. Beachten Sie die Ränder und andere Einstellungen, die vom Designer hinzugefügt werden. Ihr XAML sieht etwa wie folgt aus. Keine Sorge, wenn er nicht genau wie folgt formatiert ist. Wir haben ihn hier verkürzt, um ihn einfacher zu gestalten.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    In den nächsten Schritten aktualisieren Sie diese Werte.

4. Ändern Sie im Eigenschaftenbereich den Namenswert des TextBlock von **TextBlock** in **TitleTextBlock**. (Stellen Sie sicher, dass der TextBlock noch das aktive Objekt ist.)

5. Ändern Sie unter **Common** den Textwert in **Collection**.

    ![TextBlock-Eigenschaften](images/xaml-basics/text-block-properties.png)

    Im XAML-Editor sieht der XAML-Code nun wie folgt aus.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. Um den TextBlock zu positionieren, sollten Sie zunächst die Eigenschaftswerte entfernen, die von Visual Studio hinzugefügt wurden. Klicken Sie in der Dokumentgliederung mit der rechten Maustaste auf **TitleTextBlock** und wählen Sie dann **Layout > Alle zurücksetzen** aus.

![Dokumentgliederung](images/xaml-basics/doc-outline-reset.png)

7. Geben Sie im Eigenschaftenbereich **Margin** in das Suchfeld ein, um die **Margin**-Eigenschaft einfach aufzufinden. Legen Sie den linken und unteren Rand auf 24 Pixel fest.

    ![TextBlock-Ränder](images/xaml-basics/margins.png)

    Ränder bieten die einfachste Positionierung eines Elements auf der Seite. Sie eignen sich für die Feinabstimmung des Layouts, aber das Verwenden großer Randwerte wie die von Visual Studio hinzugefügten Werte, machen es schwieriger, Ihre Benutzeroberfläche an verschiedene Bildschirmgrößen anzupassen, und sollten daher vermieden werden.

    Weitere Informationen hierzu finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md).

8. Klicken Sie in der Dokumentgliederung mit der rechten Maustaste auf **TitleTextBlock** und wählen Sie dann **Edit Style > Apply Resource > TitleTextBlockStyle** aus. Dadurch wird ein vom System definiertes Format auf den Titeltext angewendet.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. Geben Sie im Eigenschaftenbereich **textwrapping** in das Suchfeld ein, um die **TextWrapping**-Eigenschaft einfach aufzufinden. Klicken Sie auf den _Eigenschaftenmarker_ für die **TextWrapping**-Eigenschaft, um das zugehörige Menü zu öffnen. (Der _Eigenschaftenmarker_ ist das kleine rechteckige Symbol rechts neben jedem Eigenschaftswert. Die _Eigenschaftenmarker_ ist schwarz, um anzugeben, dass die Eigenschaft auf einen nicht standardmäßigen Wert festgelegt ist.) Wählen Sie im Menü **Property** die Option **Reset** aus, um die TextWrapping-Eigenschaft zurückzusetzen.

    Visual Studio fügt diese Eigenschaft hinzu, aber sie ist bereits in dem Stil festgelegt, den Sie angewendet haben, und daher ist sie hier überflüssig.

Sie haben Ihrer App den ersten Teil der Benutzeroberfläche hinzugefügt! Führen Sie die App jetzt aus, um herauszufinden, wie sie aussieht.

Haben Sie bemerkt, dass Ihre App im XAML-Designer weißen Text auf schwarzem Hintergrund zeigt, aber wenn sie ausgeführt wird, schwarzen Text auf weißem Hintergrund anzeigt. Der Grund dafür ist, dass Windows über ein dunkles und ein helles Design verfügt, und das Standarddesign je nach Gerät variiert. Auf einem PC ist das Standarddesign hell. Sie können auf das Zahnradsymbol oben im XAML-Designer klicken, um die Geräteeinstellungen für die Vorschau zu öffnen und das Design auf die helle Einstellung setzen, damit die App im XAML-Designer genauso wie auf Ihrem PC aussieht.

> [!NOTE]
> In diesem Teil des Lernprogramms haben Sie ein Steuerelement durch Ziehen und Ablegen hinzugefügt. Sie können ebenfalls ein Steuerelement hinzufügen, indem Sie auf die Toolbox doppelklicken. Probieren Sie es aus, und sehen Sie die Unterschiede im XAML-Code, den Visual Studio generiert.

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>Teil 2: Hinzufügen eines GridView-Steuerelements mit dem XAML-Editor

In Teil 1 haben Sie einen Einblick in die Verwendung von XAML-Designer und einige der von Visual Studio bereitgestellten Tools erhalten. Hier verwenden Sie den XAML-Editor, um direkt mit XAML-Markup zu arbeiten. Wenn Sie mit XAML vertraut sind, halten Sie diese Arbeitsweise möglicherweise für effizienter.

Ersetzen Sie zuerst das Stammlayout-[Raster](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) mit einem [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). Das RelativePanel erleichtert die Neuanordnung der Benutzeroberfläche relativ zum Bereich oder anderen Teilen der Benutzeroberfläche. Den Vorteil sehen Sie im Lernprogramm [Adaptives XAML-Layout](xaml-basics-adaptive-layout.md). 

Fügen Sie dann ein [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview)-Steuerelement zum Anzeigen Ihrer Daten hinzu.

**Hinzufügen eines Steuerelements mit dem XAML-Editor**

1. Ändern Sie im XAML-Editor das Stamm-**Raster** in ein **RelativePanel**.

    **Vorher**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **Nachher**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    Weitere Informationen zum Layout mit einem **RelativePanel** finden Sie unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Fügen Sie unterhalb des **TextBlock**-Elements ein **GridView-Steuerelements** mit dem Namen „ImageGridView“ hinzu. Legen Sie die _angefügten Eigenschaften_ des **RelativePanel** fest, um das Steuerelement unter dem Titeltext zu platzieren, und geben Sie an, dass es sich über die gesamte Bildschirmbreite erstrecken soll.

    **Diesen XAML-Code hinzufügen**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Nach dem TextBlock**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    Weitere Informationen zu angefügten Paneleigenschaften finden Sie unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Damit in **GridView** Elemente angezeigt werden, müssen Sie ihm eine Sammlung von Daten hinzufügen, die angezeigt werden können. Öffnen Sie „MainPage.xaml.cs“, und suchen Sie die **GetItemsAsync**-Methode. Diese Methode füllt eine Sammlung mit dem Namen „Bilder” auf, eine Eigenschaft, die wir unter MainPage hinzugefügt haben.

    Fügen Sie nach der **foreach**-Schleife in **GetItemsAsync** folgende Codezeile hinzu.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    Dadurch wird die **ItemsSource**-Eigenschaft von GridView auf die **Images**-Sammlung der App festgelegt und es gibt **GridView**-Elemente, die angezeigt werden können.

Dies ist ein guter Ausgangspunkt zum Ausführen der App. Stellen Sie sicher, dass alles funktioniert. Sie sieht etwa wie folgt aus.

![App-UI-Prüfpunkt 1](images/xaml-basics/layout-0.png)

Sie werden feststellen, dass in der App noch keine Bilder angezeigt werden. Standardmäßig wird der ToString-Wert des Datentyps, der sich in der Sammlung befindet, angezeigt. Als Nächstes erstellen Sie eine Datenvorlage, die definiert, wie die Daten angezeigt werden.

> [!NOTE]
> Weitere Informationen zu Layouts mit einem **RelativePanel** finden Sie im Artikel [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Informieren Sie sich, und experimentieren Sie dann mit einigen anderen Layouts, indem Sie die angefügten Eigenschaften des RelativePanel auf **TextBlock** und **GridView** festlegen.

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>Teil 3: Hinzufügen einer DataTemplate zum Anzeigen Ihrer Daten

Erstellen Sie jetzt eine [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate), die GridView mitteilt, wie Ihre Daten angezeigt werden. Eine ausführliche Erläuterung zu Datenvorlagen finden Sie unter [Elementcontainer und Vorlagen](../controls-and-patterns/item-containers-templates.md).

Zunächst müssen Sie nur Platzhalter hinzufügen, die Ihnen beim Erstellen des gewünschten Layouts helfen. Im Lernprogramm [XAML-Datenbindung](../../data-binding/xaml-basics-data-binding.md) können Sie diese Platzhalter durch echte Daten aus der **ImageFileInfo**-Klasse ersetzen. Sie können jetzt die ImageFileInfo.cs-Datei öffnen, wenn Sie überprüfen möchten, wie das Datenobjekt aussieht.

**Hinzufügen einer Datenvorlage zu einer Rasteransicht**

1. Öffnen Sie „MainPage.xaml“.

2. Wenn Sie die Bewertung anzeigen möchten, verwenden Sie das **RadRating**-Steuerelement aus dem [Telerik UI für UWP](https://github.com/telerik/UI-For-UWP)-NuGet-Paket. Fügen Sie einen XAML-Namespace-Verweis hinzu, der den Namespace für das Telerik-Steuerelemente angibt. Fügen Sie ihn dem Starttag **Seite** hinzu, direkt nach den anderen 'Xmlns:'-Einträgen.

    **Diesen XAML-Code hinzufügen**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Nach dem letzten ' Xmlns:'-Eintrag**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    Weitere Informationen zu XAML-Namespaces finden Sie unter [XAML-Namespaces und Namespacezuordnung](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-namespaces-and-namespace-mapping).

3. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wählen Sie im Kontextmenü die Option **Weitere Vorlagen bearbeiten > Generierten Inhalt bearbeiten (ItemTemplate) > Leere ... erstellen** . Das Dialogfeld **Ressource erstellen** wird geöffnet.

4. Ändern Sie im Dialogfeld den Wert des Namens (Schlüssel) in **ImageGridView_DefaultItemTemplate**, und klicken Sie dann auf **OK**.

    Folgendes geschieht, wenn Sie auf **OK** klicken.

    - Dem Abschnitt Page.Resources von „MainPage.xaml“ wird eine **DataTemplate** hinzugefügt.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - Der Dokumentgliederungsbereich wird auf diese **DataTemplate** festgelegt.

        Wenn Sie die Datenvorlage erstellt haben, klicken Sie auf den Pfeil in der oberen linken Ecke der Dokumentgliederung, um zum Seitenbereich zurückzukehren.

    - Die **ItemTemplate**-Eigenschaft der GridView wird auf die **DataTemplate**-Ressource festgelegt.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. Weisen Sie in der **ImageGridView_DefaultItemTemplate**-Ressource dem Stamm-**Raster** eine Höhe und Breite von **300** sowie einen Rand von **8** zu. Fügen Sie dann zwei Zeilen hinzu, und legen Sie die Höhe der zweiten Zeile auf **Auto** fest.

    **Vorher**
    ```xaml
    <Grid/>
    ```

    **Nachher**
    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    Weitere Informationen zu Rasterlayouts finden Sie unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid).

6. Hinzufügen von Steuerelementen zum Raster

    a. Fügen Sie ein **Image**-Steuerelement in die erste Rasterzeile ein. Hier wird das Bild angezeigt, doch im Moment können Sie das Logo des App Stores als Platzhalter verwenden.

    b. Fügen Sie **TextBlock**-Steuerelemente zum Anzeigen des Namens, Dateityps und der Dimensionen des Bilds hinzu. Verwenden Sie hierzu **StackPanel**-Steuerelemente, um die Textblöcke anzuordnen.

    Weitere Informationen zu **StackPanel**-Layouts finden Sie unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)

    c. Hinzufügen des **RadRating**-Steuerelements zum äußeren (vertikalen) **StackPanel**. Platzieren Sie es nach dem inneren (horizontalen) **StackPanel**.

    **Die endgültige Vorlage**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <telerikInput:RadRating Value="3"
                                    IsReadOnly="True">
                <telerikInput:RadRating.FilledIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="SolidStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.FilledIconContentTemplate>
                <telerikInput:RadRating.EmptyIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="OutlineStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.EmptyIconContentTemplate>
            </telerikInput:RadRating>

        </StackPanel>
    </Grid>
    ```

Führen Sie die App jetzt aus, um die **GridView** mit der soeben erstellten Elementvorlage anzuzeigen. Möglicherweise wird das Bewertungssteuerelement nicht angezeigt, da es weiße Sterne auf weißem Hintergrund aufweist. Im nächsten Schritt ändern Sie die Hintergrundfarbe.

![App-UI-Prüfpunkt 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Teil 4: Ändern des Elementcontainerstils

Eine Steuerelementvorlage für ein Element enthält die optischen Elemente zur Anzeige des Zustands wie Auswahl, Draufzeigen und Fokus. Diese visuellen Elemente werden über oder unter der Datenvorlage gerendert. Hier ändern Sie die **Background**- und **Margin**-Eigenschaften der Steuerelementvorlage, um dem **GridView**-Elemente einen grauen Hintergrund hinzuzufügen.

**Ändern des Elementcontainers**

1. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wählen Sie im Kontextmenü **Weitere Vorlagen bearbeiten > Erzeugten Elementcontainer bearbeiten (ItemContainerStyle) > Kopie bearbeiten** aus. Das Dialogfeld **Ressource erstellen** wird geöffnet.

2. Ändern Sie im Dialogfeld den Wert des Namens (Schlüssel) in **ImageGridView_DefaultItemContainerStyle**, und klicken Sie anschließend auf **OK**.

    Dem Abschnitt **Page.Resources** des XAML-Codes wird eine Kopie des Standardstils hinzugefügt.

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="True"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="FocusVisualPrimaryBrush" Value="{ThemeResource GridViewItemFocusVisualPrimaryBrush}"/>
        <Setter Property="FocusVisualPrimaryThickness" Value="2"/>
        <Setter Property="FocusVisualSecondaryBrush" Value="{ThemeResource GridViewItemFocusVisualSecondaryBrush}"/>
        <Setter Property="FocusVisualSecondaryThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    Der **GridViewItem**-Standardstil legt zahlreiche Eigenschaften fest. Es wird empfohlen, mit einer Kopie des Standardstils zu beginnen und nur die notwendigen Eigenschaften zu ändern. Andernfalls werden die visuellen Elemente wahrscheinlich nicht so angezeigt, wie Sie es erwarten, da einige Eigenschaften nicht richtig festgelegt sind.

    Und wie im vorherigen Schritt wird die **ItemContainerStyle**-Eigenschaft der GridView auf die neue **Stil**-Ressource festgelegt.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. Ändern Sie den Wert der **Background**-Eigenschaft in **Gray**.

    **Vorher**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **Nachher**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. Ändern Sie den Wert der **Margin**-Eigenschaft in **8**.

    **Vorher**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **Nachher**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

Führen Sie die App aus, um herauszufinden, wie sie nun aussieht. Ändern Sie die Größe des App-Fensters. Die **GridView** kümmert sich um das Neuanordnen der Bilder; bei einigen Breiten ist jedoch noch viel Platz auf der rechten Seite des App-Fensters vorhanden. Es würde besser aussehen, wenn die Bilder zentriert werden. Damit werden wir uns als Nächstes befassen.

![App-UI-Prüfpunkt 3](images/xaml-basics/layout-2.png)

> [!Note]
> Wenn Sie experimentieren möchten, versuchen Sie, die Background- und Margin-Eigenschaften mit unterschiedlichen Werten festzulegen, und stellen Sie fest, welche Auswirkung dies hat.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Teil 5: Anwenden einiger abschließender Anpassungen am Layout

Um die Bilder auf der Seite zu zentrieren, müssen Sie die Ausrichtung des Rasters auf der Seite anpassen. Oder möchten Sie die Ausrichtung der Bilder in der **GridView** anpassen? Ist es wichtig? Mal sehen.

Weitere Informationen zur Ausrichtung finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md).

(Sie können in diesem Schritt versuchen, die Einstellung der **Background**-Eigenschaft der **GridView** auf Ihre bevorzugte Farbe festzulegen. So sehen Sie deutlicher, was mit dem Layout passiert.)

**Ändern der Ausrichtung der Bilder**

1. Legen Sie in der **Gridview** die **HorizontalAlignment**-Eigenschaft auf **Center** fest.

    **Vorher**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **Nachher**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}" 
                  HorizontalAlignment="Center"/>
    ```

2. Führen Sie die App aus, und ändern Sie die Größe des Fensters. Scrollen Sie nach unten, um weitere Bilder anzuzeigen.

    Die Bilder werden zentriert, was besser aussieht. Die Bildlaufleiste wird jedoch am Rand von **GridView** und nicht am Rand des Fensters ausgerichtet. Um dieses Problem zu beheben, zentrieren Sie die Bilder in der **GridView**, anstatt die **GridView** auf der Seite zu zentrieren. Es ist etwas mehr Arbeit, sieht aber am Ende besser aus.

3. Entfernen Sie die **HorizontalAlignment**-Einstellung aus dem vorherigen Schritt.

4. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wählen Sie im Kontextmenü **Weitere Vorlagen bearbeiten > Layout der Elemente bearbeiten (ItemsPanel) > Kopie bearbeiten...** aus. Das Dialogfeld **Ressource erstellen** wird geöffnet.

5. Ändern Sie im Dialogfeld den Wert des Namens (Schlüssel) in **ImageGridView_ItemsPanelTemplate**, und klicken Sie dann auf **OK**.

    Eine Kopie der Standard-**ItemsPanelTemplate** wird dem Abschnitt **Page.Resources** im XAML-Code hinzugefügt. (Wie zuvor, wird die **GridView** aktualisiert, um auf diese Ressource zu verweisen.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    Genau wie die verschiedenen Bereiche für das Layout der Steuerelemente in Ihrer App verwendet wurden, enthält die **GridView** einen internen Bereich, in dem das Layout der Elemente verwaltet wird. Nun da Sie Zugriff auf diesen Bereich haben (das **ItemsWrapGrid**), können Sie die Eigenschaften des Layouts der Elemente in der **GridView** ändern.

6. Legen Sie in **ItemsWrapGrid** die **HorizontalAlignment**-Eigenschaft auf **Center** fest.

    **Vorher**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **Nachher**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. Führen Sie die App aus, und ändern Sie erneut die Größe des Fensters. Scrollen Sie nach unten, um weitere Bilder anzuzeigen.

![App-UI-Prüfpunkt 4](images/xaml-basics/layout-3.png)

Nun ist die Bildlaufleiste am Rand des Fensters ausgerichtet. Gut gemacht! Sie haben die grundlegende Benutzeroberfläche für Ihre App erstellt.

## <a name="going-further"></a>Vertiefung

Nachdem Sie nun die grundlegende Benutzeroberfläche erstellt haben, können Sie diese weiteren Tutorials nutzen, die ebenfalls auf dem PhotoLab-Beispiel basieren: 

* Fügen Sie echte Bilder und Daten im [XAML-Datenbindungs-Tutorial](../../data-binding/xaml-basics-data-binding.md) hinzu.
* Passen Sie die Benutzeroberfläche im [XAML-Layout-Tutorial](xaml-basics-adaptive-layout.md) an unterschiedliche Bildschirmgrößen an.


## <a name="get-the-final-version-of-the-photolab-sample"></a>Abrufen der endgültigen Version des PhotoLab-Beispiels

In diesem Lernprogramm wird nicht das Erstellen der kompletten Fotobearbeitungs-App erläutert. Lesen Sie daher die [endgültige Version](https://github.com/Microsoft/Windows-appsample-photo-lab) zu weiteren Features wie z. B. benutzerdefinierte Animationen und Support für Ihr Smartphone.

