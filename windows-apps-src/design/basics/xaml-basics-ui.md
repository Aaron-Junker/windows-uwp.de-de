---
title: Erstellen einer einfachen Benutzeroberfläche – Tutorial
description: In diesem Artikel werden die Grundlagen für Erstellen einer Benutzeroberfläche in XAML behandelt
keywords: XAML, UWP, Erste Schritte
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1bae8455f1062b3ad62aeac3807c6c58ae274a1b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640815"
---
# <a name="tutorial-create-a-user-interface"></a>Tutorial: Erstellen einer Benutzeroberfläche

In diesem Tutorial lernen Sie, wie Sie eine grundlegende Benutzeroberfläche für ein Bildbearbeitungsprogramm erstellen: 

+ Unter Verwendung des XAML-Tools in Visual Studio, z. B. XAML-Designer, Toolbox, XAML-Editor Eigenschaftenpanel und Dokumentgliederung können Sie Steuerelemente und Inhalte auf der Benutzeroberfläche hinzufügen
+ Unter Verwendung einiger der häufigsten XAML-Layoutpanels, z. B. RelativePanel, Grid und StackPanel-Element.

Das Bildbearbeitungsprogramm hat zwei Seiten/Bildschirme:

Die **Hauptseite** zeigt eine Ansicht der Foto-Galerie, zusammen mit einigen Informationen über jede Bilddatei an.

![MainPage](images/xaml-basics/mainpage.png)

Die **Detailsseite** zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio 2017: [Visual Studio 2017 Community (kostenlos) herunterladen](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK (10.0.15063.468 oder höher):  [Das aktuelle Windows-SDK (kostenlos) herunterladen](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Den Startcode von Github abrufen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels. 

1. Wechseln Sie zu [ https://github.com/Microsoft/Windows-appsample-photo-lab ](https://github.com/Microsoft/Windows-appsample-photo-lab). Sie gelangen auf die GitHub-Seite für das Beispiel. 
2. Als nächstes müssen Sie das Beispiel klonen oder herunterladen. Klicken Sie auf die Schaltfläche **Clone or download**. Ein Untermenü erscheint.
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Das <b>Clone or download</b>-Menü auf der GitHub-Seite des Fotolbeispiels.</figcaption>
    </figure>

    **Wenn Sie nicht mit GitHub vertraut sind:**
    
    a. Klicken Sie auf **Download ZIP** und speichern Sie die Datei lokal. Es wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.
    b. Entpacken Sie die Datei. Verwenden Sie den Datei-Explorer, um zu der gerade heruntergeladenen ZIP-Datei zu navigieren, klicken Sie mit der rechten Maustaste darauf und wählen Sie **Alle extrahieren...** aus. c. Navigieren Sie zu Ihrer lokalen Kopie des Beispiels und wechseln Sie zum `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`-Verzeichnis.    

    **Wenn Sie mit GitHub vertraut sind:**

    a. Klonen Sie den Master-Branch des Repos lokal.
    b. Navigieren Sie zum `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`-Verzeichnis.

3. Öffnen Sie das Projekt durch einen Klick auf `Photolab.sln`.

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>Teil 1: Fügen Sie ein TextBlock-Element mit dem XAML-Designer

Visual Studio bietet verschiedene Tools zum leichteren Erstellen der XAML-UI. Mit XAML-Designer können Sie Steuerelemente auf die Designoberfläche ziehen und vor dem Ausführen der App anschauen. Mit dem Eigenschaftenpanel können Sie alle Eigenschaften des Steuerelements, die im Designer aktiv sind, ansehen und einstellen. Die Dokumentgliederung zeigt die über- und untergeordnete Struktur der visuellen XAML-Struktur für Ihre Benutzeroberfläche. Mit dem XAML-Editor können Sie das XAML-Markup direkt eingeben und ändern.

Hier ist die Visual Studio-UI mit den bezeichneten Tools.

![Layout in Visual Studio](images/xaml-basics/visual-studio-tools.png)

Diese Tools helfen Ihnen beim einfachen Erstellen der Benutzeroberfläche, deshalb werden sie alle in diesem Lernprogramm verwendet. Beginnen Sie mit XAML-Designer zum Hinzufügen eines Steuerelements. 

**Fügen Sie ein Steuerelement mit dem XAML-Designer hinzu:**

1. Doppelklicken Sie im Projektmappen-Explorer auf die Datei **MainPage.xaml**, um sie zu öffnen. Dies zeigt die Hauptseite der App an, ohne das ein UI-Element hinzugefügt wurde.

2. Vor dem fortfahren müssen Sie einige Anpassungen zu Visual Studio vornehmen.

    - Stellen Sie sicher, dass Ihre Projektmappen-Plattform auf x86 oder x64 und nicht ARM festgelegt ist.
    - Legen Sie auf der Hauptseite des XAML-Designer die Desktopvorschau auf 13,3" fest.

    Beide Einstellungen sollten im oberen Bereich des Fensters angezeigt werden, wie hier gezeigt.

    ![VS-Einstellungen:](images/xaml-basics/layout-vs-settings.png)

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

4. Ändern Sie im Bereich Eigenschaften den Namenswert des TextBlock von **TextBlock** auf **TitleTextBlock**. (Stellen Sie sicher, dass der TextBlock noch das aktive Objekt ist.)

5. Unter **Common**, ändern Sie den Textwert zu **Collection**.

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

6. Um den TextBlock zu positionieren, sollten Sie zunächst die Eigenschaftswerte entfernen, die von Visual Studio hinzugefügt wurden. Klicken Sie unter Dokumentgliederung mit der rechten Maustaste auf **TitleTextBlock** und wählen Sie dann **Layout > Alle zurücksetzen** aus.

![Dokumentgliederung](images/xaml-basics/doc-outline-reset.png)

7. Geben Sie im Bereich Eigenschaften **Margin** in das Suchfeld ein, um die Eigenschaft der **Margin** ganz leicht zu finden. Legen Sie den linken und unteren Rand auf 24 Pixel fest.

    ![TextBlock-Ränder](images/xaml-basics/margins.png)

    Ränder bieten die einfachste Positionierung eines Elements auf der Seite. Sie eignen sich für die Feinabstimmung des Layouts, aber das Verwenden großer Randwerte wie die von Visual Studio hinzugefügten Werte, machen es für Ihre Benutzeroberfläche schwieriger, sich an verschiedene Bildschirmgrößen anzupassen und sollten daher vermieden werden.

    Weitere Informationen hierzu finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md).

8. Klicken Sie mit der rechten Maustaste auf **TitleTextBlock** und wählen Sie dann **Edit Style > Apply Resource > TitleTextBlockStyle**. Dies wendet ein vom System definiertes Format für den Titeltext an.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. Geben Sie im Bereich Eigenschaften **textwrapping** in das Suchfeld ein, um die Eigenschaft der **TextWrapping** ganz leicht zu finden. Klicken Sie auf den _Eigenschaftsmarker_ für den **TextWrapping**, um das Menü zu öffnen. (Der _Eigenschaftsmarker_ ist das kleine rechteckige Symbol rechts neben jedem Eigenschaftswert. Die _Eigenschaftenmarker_ ist schwarz, um anzugeben, dass die Eigenschaft auf einen nicht standardmäßigen Wert festgelegt ist.) Auf der **Eigenschaft** , wählen Sie im Menü **zurücksetzen** zum Zurücksetzen der TextWrapping-Eigenschaft.

    Visual Studio fügt diese Eigenschaft hinzu, aber sie ist bereits in der Formatvorlage festgelegt, die Sie angewendet haben und Sie ist hier überflüssig.

Sie haben Ihrer App den ersten Teil der Benutzeroberfläche hinzugefügt! Führen Sie die App jetzt aus, um herauszufinden, wie sie aussieht.

Haben Sie bemerkt, dass Ihre App im XAML-Designer weißen Text auf schwarzem Hintergrund zeigt, aber wenn sie ausgeführt wird, schwarzen Text auf weißem Hintergrund anzeigt. Der Grund dafür ist, dass Windows über ein dunkles und ein helles Design verfügt, und das Standarddesign je nach Gerät variiert. Auf einem PC ist das Standarddesign hell. Sie können das Zahnradsymbol oben im XAML-Designer anklicken, um die Geräteeinstellungen für die Vorschau zu öffnen und das Design auf ändern setzen, damit die App im XAML-Designer genauso wie auf Ihrem PC aussieht.

> [!NOTE]
> In diesem Teil des Lernprogramms haben Sie ein Steuerelement durch Ziehen und Ablegen hinzugefügt. Sie können ebenfalls ein Steuerelement hinzufügen, indem Sie auf die Toolbox doppelklicken. Probieren Sie es aus, und sehen Sie die Unterschiede im XAML-Code, den Visual Studio generiert.

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>Teil 2: Hinzufügen einer GridView-Steuerelement, das mit dem XAML-editor

In Teil 1 haben Sie einen Einblick auf die Verwendung von XAML-Designer und einige der von Visual Studio bereitgestellten Tools erhalten. Hier verwenden Sie den XAML-Editor, um direkt mit XAML-Markup zu arbeiten. Wenn Sie mit XAML vertraut sind, finden Sie unter Umständen, dass es sich um eine effiziente Art und Weise der Arbeit handelt.

Ersetzen Sie zuerst das Stammlayout-[Raster](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) mit einem [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). Das RelativePanel erleichtert die Neuanordnung der Benutzeroberfläche relativ zum Bereich oder anderen Teilen der Benutzeroberfläche. Sie sehen den Nutzens im Lernprogramm [Adaptives XAML-Layout](xaml-basics-adaptive-layout.md). 

Fügen Sie dann ein [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview)-Steuerelement zum Anzeigen Ihrer Daten hinzu.

**Hinzufügen eines Steuerelements, die mit dem XAML-editor**

1. Ändern Sie im XAML-Editor das Stamm-**Raster** zu einem **RelativePanel**.

    **Vor dem**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **After**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    Weitere Informationen zum Layout mit einem **RelativePanel** finden Sie unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Fügen Sie unterhalb des **TextBlock**-Elements ein **GridView-Steuerelements** mit dem Namen 'ImageGridView' hinzu. Legen Sie die **"relativepanel"** _angefügte Eigenschaften_ platzieren Sie das Steuerelement unterhalb des Titeltexts und die gesamte Breite des Bildschirms erstreckt.

    **Fügen Sie diesem XAML hinzu.**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Nach dem TextBlock-Element**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    Weitere Informationen zu Angefügte Paneleigenschaften finden Sie unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Damit **GridView** Elemente anzeigt, müssen Sie ihm eine Sammlung von Daten zum Anzeigen hinzufügen. Öffnen Sie MainPage.xaml.cs und suchen Sie die **GetItemsAsync**-Methode. Diese Methode füllt eine Sammlung mit dem Namen „Bilder” auf, was eine Eigenschaft ist, die wir unter MainPage hinzugefügt haben.

    Fügen Sie nach der **foreach**-Schleife in **GetItemsAsync** folgende Codezeile hinzu.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    Dadurch wird die GridView **ItemsSource**-Eigenschaft auf die **Bilder**-Sammlung der App festgelegt und es gibt Elemente **GridView**, die angezeigt werden können.

Dies ist ein guter Ausgangspunkt zum Ausführen der App. Stellen Sie sicher, dass alles funktioniert. Es sieht etwa wie folgt aus.

![App-UI-Prüfpunkt 1](images/xaml-basics/layout-0.png)

Sie werden feststellen, dass in der App noch keine Bilder angezeigt werden. Standardmäßig wird der ToString-Wert des Datentyps, der sich in der Auflistung befindet, angezeigt. Als Nächstes erstellen Sie eine Vorlage, die definiert, wie die Daten angezeigt werden.

> [!NOTE]
> Sie finden weitere Informationen zur Verwendung des Layouts eines **RelativePanel** im Artikel [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Schauen Sie mal nach und experimentieren Sie dann mit einigen anderen Layouts, indem Sie die angefügten Eigenschaften des RelativePanel auf **TextBlock** und **GridView** festlegen.

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>Teil 3: Hinzufügen von einem DataTemplate zum Anzeigen Ihrer Daten

Erstellen Sie jetzt ein [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate), das GridView mitteilt, wie Ihre Daten angezeigt werden. Eine ausführliche Erläuterung der Datenvorlagen finden Sie unter [Elementcontainer und Vorlagen](../controls-and-patterns/item-containers-templates.md).

Zunächst müssen Sie nur Platzhalter hinzufügen, die Ihnen beim Erstellen des gewünschten Layouts helfen. Im Lernprogramm [XAML-Datenbindung](../../data-binding/xaml-basics-data-binding.md) können Sie diese Platzhalter durch echte Daten aus der **ImageFileInfo**-Klasse ersetzen. Sie können jetzt die ImageFileInfo.cs-Datei öffnen, wenn Sie überprüfen möchten, wie das Datenobjekt aussieht.

**Fügen Sie eine Vorlage in eine Rasteransicht**

1. Öffnen Sie MainPage.xaml.

2. Wenn die Bewertung anzeigen möchten, verwenden Sie das **RadRating**-Steuerelement aus dem [Telerik UI für UWP](https://github.com/telerik/UI-For-UWP)-NuGet-Paket. Fügen Sie einen XAML-Namespace-Verweis hinzu, der den Namespace für das Telerik-Steuerelemente angibt. Fügen Sie ihn dem Starttag **Seite** hinzu, direkt nach den anderen ' Xmlns:'-Einträgen.

    **Fügen Sie diesem XAML hinzu.**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Nach dem letzten "Xmlns:' Eintrag**

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

3. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wählen Sie im Kontextmenü des **zusätzliche Vorlagen bearbeiten > generierte Elemente bearbeiten (ItemTemplate) > leere erstellen...** . Die **Ressource erstellen** Dialogfeld wird geöffnet.

4. Ändern Sie im Dialogfeld den Wert des Namens (Schlüssel) zu **ImageGridView_DefaultItemTemplate** und klicken Sie dann auf **OK**.

    Folgendes passiert, wenn Sie auf **OK** klicken.

    - Es wird ein **DataTemplate** dem Abschnitt Page.Resources auf MainPage.xaml hinzugefügt.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - Der Dokumentgliederungsbereicht wird auf **DataTemplate** festgelegt.

        Wenn Sie die Datenvorlage erstellt haben, klicken Sie auf den Pfeil in der oberen linken Ecke der Dokumentgliederung, um zum Seitenbereich zurückzukehren.

    - Die GridView **ItemTemplate**-Eigenschaft wird auf die **DataTemplate**-Ressource festgelegt.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. In der **ImageGridView_DefaultItemTemplate**-Ressource, geben Sie dem Stamm-**Raster** eine Höhe und Breite von **300**, und einen Rand von **8**. Fügen Sie dann zwei Zeilen hinzu und legen Sie die Höhe der zweiten Zeile auf **Auto** fest.

    **Vor dem**
    ```xaml
    <Grid/>
    ```

    **After**
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

6. Hinzufügen von Steuerelementen zum Raster.

    a. Fügen Sie ein **Bild**-Steuerelement in die erste Rasterzeile ein. Hier wird das Bild angezeigt, doch im Moment können Sie das Logo des App Stores als Platzhalter verwenden.

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

Führen Sie die App jetzt aus, um **GridView** mit der Elementvorlage anzuzeigen, die Sie gerade erstellt haben. Möglicherweise wird das Bewertungssteuerelement nicht angezeigt, da es weiße Sternen auf weißem Hintergrund hat. Als Nächstes ändern Sie die Hintergrundfarbe.

![App-UI-Prüfpunkt 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Teil 4: Ändern des Stils der Element-container

Eine Steuerelementvorlage für ein Element enthält die optischen Elemente zur Anzeige des Zustands wie Auswahl, Draufzeigen und Fokus. Diese visuellen Elemente werden über oder unter der Datenvorlage gerendert. Hier ändern Sie die **Hintergrund**- und **Rand**-Eigenschaften der Steuerelementvorlage, um dem **GridView**-Elemente einen grauen Hintergrund hinzuzufügen.

**Ändern Sie den Elementcontainer**

1. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wählen Sie im Kontextmenü des **zusätzliche Vorlagen bearbeiten > erstellten Elementcontainer bearbeiten (ItemContainerStyle) > Kopie bearbeiten...** . Die **Ressource erstellen** Dialogfeld wird geöffnet.

2. Ändern Sie im Dialogfeld den Wert des Namens (Schlüssel) zu **ImageGridView_DefaultItemContainerStyle** und klicken Sie dann auf **OK**.

    Es wird eine Kopie des Standardstils dem Abschnitt **Page.Resources** des XAML-Codes hinzugefügt.

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

    Der **GridViewItem**-Standardstil legt zahlreiche Eigenschaften fest. Es wird empfohlen, mit einer Kopie des Standardstils zu beginnen und nur die notwendigen Eigenschaften zu ändern. Andernfalls werden die visuellen Elemente wahrscheinlich nicht so wie angezeigt, die Sie es erwarten, da einige Eigenschaften nicht richtig festgelegt sein werden.

    Und wie im vorherigen Schritt wird die GridView **ItemContainerStyle**-Eigenschaft auf die neue **Stil**-Ressource festgelegt.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. Ändern Sie den Wert der **Hintergrund**-Eigenschaft auf **Grau**.

    **Vor dem**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **After**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. Ändern Sie den Wert der **Rand**-Eigenschaft auf **8**.

    **Vor dem**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **After**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

Führen Sie die App aus, um herauszufinden, wie sie bisher aussieht. Skalieren des App-Fensters. **GridView** kümmert sich um das Neuanordnen der Bilder, aber bei einige Breiten ist noch viel Platz auf der rechten Seite des App-Fensters vorhanden. Es würde besser aussehen, wenn die Bilder zentriert werden. Damit werden wir uns als nächstes befassen.

![App-UI-Prüfpunkt 3](images/xaml-basics/layout-2.png)

> [!Note]
> Wenn Sie experimentieren möchten, versuchen Sie, die Hintergrund- und Rand-Eigenschaften mit unterschiedlichen Werten festzulegen und welche Auswirkung dies hat.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Teil 5: Wenden Sie einige abschließende Anpassungen vornehmen, auf das layout

Um die Bilder auf der Seite zu zentrieren, müssen Sie die Ausrichtung des Rasters auf der Seite anpassen. Oder möchten Sie die Ausrichtung der Bilder in **GridView** anpassen? Ist es wichtig? Mal sehen.

Weitere Informationen zur Anpassung finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md).

(Sie können in diesem Schritt versuchen, die Einstellung des **Hintergrunds** des **GridViews** auf Ihre bevorzugte Farbe festzulegen. So sehen Sie, was mit dem Layout passiert.)

**Ändern der Ausrichtung der Bilder**

1. Legen Sie in **Gridview** die **HorizontalAlignment**-Eigenschaft auf **Center** fest.

    **Vor dem**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **After**
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

2. Führen Sie die App aus, und skalieren Sie das Fenster. Scrollen Sie nach unten, um weitere Bilder anzuzeigen.

    Die Bilder werden zentriert, was besser aussieht. Die Bildlaufleiste wird jedoch am Rand von **GridView** anstelle am Rand des Fensters ausgerichtet. Um dieses Problem zu beheben, zentrieren Sie die Bilder in der **GridView**, anstatt sie in der **GridView** auf der Seite zu zentrieren. Es ist etwas mehr Arbeit, sieht aber am Ende besser aus.

3. Entfernen Sie die **HorizontalAlignment**-Einstellung aus dem vorherigen Schritt.

4. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wählen Sie im Kontextmenü des **zusätzliche Vorlagen bearbeiten > Layout der Elemente bearbeiten (ItemsPanel) > Kopie bearbeiten...** . Die **Ressource erstellen** Dialogfeld wird geöffnet.

5. Ändern Sie im Dialogfeld den Wert des Namens (Schlüssel) zu **ImageGridView_ItemsPanelTemplate** und klicken Sie dann auf **OK**.

    Es wird eine Kopie des Standard-**ItemsPanelTemplate** dem Abschnitt **Page.Resources** des XAML-Codes hinzugefügt. (Wie zuvor, wird die **GridView** aktualisiert, um auf diese Ressource zu verweisen.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    Genau wie die verschiedenen Bereiche für das Layout der Steuerelemente in Ihrer App verwendet wurden, enthält **GridView** einen internen Bereich, der das Layout der Elemente verwaltet. Jetzt wo Sie Zugriff auf diesen Bereich haben (das **ItemsWrapGrid**), können Sie die Eigenschaften des Layouts der Elementen in **GridView** ändern.

6. Legen Sie in **ItemsWrapGrid** die **HorizontalAlignment**-Eigenschaft auf **Center** fest.

    **Vor dem**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **After**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. Führen Sie die App aus, und skalieren Sie das Fenster erneut. Scrollen Sie nach unten, um weitere Bilder anzuzeigen.

![App-UI-Prüfpunkt 4](images/xaml-basics/layout-3.png)

Nun ist die Bildlaufleiste am Rand des Fensters ausgerichtet. Gut gemacht! Sie haben die grundlegende Benutzeroberfläche für Ihre App erstellt.

## <a name="going-further"></a>Vertiefung

Nachdem Sie nun die grundlegende Benutzeroberfläche erstellt haben, können Sie diese weitere Tutorials nutzen, die ebenfalls auf dem PhotoLab-Beispiel basieren: 

* Fügen Sie echte Bilder und Daten im [XAML-Datenbindungs-Tutorial](../../data-binding/xaml-basics-data-binding.md) hinzu.
* Passen Sie die Benutzeroberfläche im [XAML-Layout-Tutorial](xaml-basics-adaptive-layout.md) an unterschiedliche Bildschirmgrößen an.


## <a name="get-the-final-version-of-the-photolab-sample"></a>Abrufen der finalen Version des PhotoLab-Beispiels

In diesem Lernprogramm lernen Sie nicht das Erstellen der vollständigen App. Lesen Sie daher die [endgültige Version](https://github.com/Microsoft/Windows-appsample-photo-lab) zu Features durch wie z. B. benutzerdefinierte Animationen und Support für Ihre Telefon.

