---
title: Erstellen einer Benutzeroberfläche – Tutorial
description: In diesem Artikel werden die Grundlagen beim Erstellen von Benutzeroberflächen in XAML beschrieben.
keywords: XAML, UWP, Erste Schritte
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c23a9539d0fc3902f715917b380e8b6b3e132c15
ms.sourcegitcommit: 1d868968297d0d6d02cc38fe84d0a3ab5bccfb60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68974453"
---
# <a name="tutorial-create-a-user-interface"></a>Tutorial: Erstellen einer Benutzeroberfläche

In diesem Tutorial erfahren Sie, wie Sie eine einfache Benutzeroberfläche für ein Bildbearbeitungsprogramm erstellen: 

+ Unter Verwendung der XAML-Tools in Visual Studio, z.B. XAML-Designer, Toolbox, XAML-Editor, Bereich **Eigenschaften** und Dokumentgliederung, zum Hinzufügen von Steuerelementen und Inhalten in der Benutzeroberfläche.
+ Unter Verwendung einiger der häufigsten XAML-Layoutpanels, z.B. **RelativePanel**, **Grid** und **StackPanel**.

Das Bildbearbeitungsprogramm umfasst zwei Seiten. Die *Hauptseite* zeigt eine Ansicht der Foto-Galerie zusammen mit einigen Informationen über jede Bilddatei an.

![Hauptseite](images/xaml-basics/mainpage.png)

Die *Detailseite* zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![Detailseite](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio 2019: [Visual Studio 2019 Community herunterladen (kostenlos)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK (10.0.15063.468 oder höher):  [Das aktuelle Windows SDK herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Startcode von GitHub abrufen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels. 

1. Navigiere zur [GitHub-Seite für das Beispiel](https://github.com/Microsoft/Windows-appsample-photo-lab). 
2. Als Nächstes müssen Sie das Beispiel klonen oder herunterladen. Wähle die Schaltfläche **Clone or download** (Klonen oder Herunterladen) aus. Ein Untermenü wird angezeigt.
    ![Das Menü „Clone or download“ (Klonen oder Herunterladen) auf der GitHub-Seite des PhotoLab-Beispiels](images/xaml-basics/clone-repo.png)
    
    **Wenn Sie mit GitHub nicht vertraut sind:**
    
    a. Wähle **Download ZIP** aus, und speichere die Datei lokal. Mit diesem Schritt wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.

    b. Entpacken Sie die Datei. Verwende den Datei-Explorer, um zu der gerade heruntergeladenen ZIP-Datei zu navigieren, klicke mit der rechten Maustaste darauf, und wähle **Alle extrahieren** aus.

    c. Navigiere zu deiner lokalen Kopie des Beispiels, und wechsle zum Verzeichnis `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.    

    **Wenn Sie mit GitHub vertraut sind:**

    a. Klonen Sie den Master-Branch des Repositorys lokal.

    b. Wechsle zum `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`-Verzeichnis.

3. Öffne das Projekt, indem du **Photolab.sln** auswählst.

## <a name="part-1-add-a-textblock-control-by-using-xaml-designer"></a>Teil 1: Hinzufügen eines TextBlock-Steuerelements mit XAML-Designer

Visual Studio bietet verschiedene Tools zum leichteren Erstellen der XAML-UI. Mit XAML-Designer kannst du Steuerelemente auf die Designoberfläche ziehen und vor dem Ausführen der App anschauen. Im Bereich **Eigenschaften** kannst du alle Eigenschaften des Steuerelements, die im Designer aktiv sind, überprüfen und festlegen. Die Dokumentgliederung zeigt die Struktur über- und untergeordneter Elemente der visuellen XAML-Struktur für deine Benutzeroberfläche. Mit dem XAML-Editor kannst du das XAML-Markup direkt eingeben und ändern.

Hier ist die Visual Studio-UI mit den bezeichneten Tools.

![Layout in Visual Studio](images/xaml-basics/visual-studio-tools.png)

Da jedes dieser Tools dir das Erstellen der Benutzeroberfläche vereinfacht, werden wir alle in diesem Tutorial verwenden. Beginnen Sie mit XAML-Designer, um ein Steuerelement hinzuzufügen. 

So fügst du ein Steuerelement mithilfe von XAML-Designer hinzu:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei „MainPage.xaml“, um sie zu öffnen. Dieser Schritt zeigt die Hauptseite der App an, ohne dass ein UI-Element hinzugefügt wird.

2. Vor dem Fortfahren musst du einige Anpassungen in Visual Studio vornehmen:

    - Stelle sicher, dass deine Projektmappenplattform auf x86 oder x64 und nicht auf ARM festgelegt ist.
    - Lege auf der Hauptseite des XAML-Designers die Desktopvorschau auf 13,3" fest.

    Beide Einstellungen sollten im oberen Bereich des Fensters angezeigt werden, wie hier veranschaulicht.

    ![Einstellungen von Visual Studio](images/xaml-basics/layout-vs-settings.png)

    Sie können die App jetzt ausführen, es wird allerdings nicht viel angezeigt. Lassen Sie uns einige UI-Elemente einfügen, um die Dinge interessanter zu gestalten.

3. Erweitern Sie in der Toolbox **Allgemeine XAML-Steuerelemente** und suchen Sie das [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock)-Steuerelement. Ziehe einen **TextBlock** auf die Designoberfläche neben der oberen linken Ecke der Seite.

    Der **TextBlock** wird zur Seite hinzugefügt, und der Designer legt einige Eigenschaften auf Grundlage seiner Einschätzung des gewünschten Layouts fest. Der **TextBlock** wird blau hervorgehoben, womit angezeigt wird, dass es sich dabei um das aktive Objekt handelt. Beachte die Ränder und andere Einstellungen, die vom Designer hinzugefügt wurden. 
    
    Dein XAML sieht etwa wie folgt aus. Mach dir keine Sorgen, wenn die Formatierung nicht genau so aussieht wie hier. Wir haben sie hier gekürzt, damit sie leichter lesbar ist.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    In den nächsten Schritten aktualisieren Sie diese Werte.

4. Ändere im Bereich **Eigenschaften** den Wert **Name** des Steuerelements **TextBlock** von **textBlock** in **TitleTextBlock**. (Stelle sicher, dass das Steuerelement **TextBlock** noch das aktive Objekt ist.)

5. Ändere unter **Common** den Wert **Text** in **Collection**.

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

6. Um den **TextBlock** zu positionieren, solltest du zunächst die von Visual Studio hinzugefügten Eigenschaftswerte entfernen. Klicke in der Dokumentgliederung mit der rechten Maustaste auf **TitleTextBlock**, und wähle dann **Layout** > **Alle zurücksetzen** aus.

    ![Dokumentgliederung](images/xaml-basics/doc-outline-reset.png)

7. Gib im Bereich **Eigenschaften** in das Suchfeld **margin** ein, um die **Margin**-Eigenschaft einfach aufzufinden. Legen Sie den linken und unteren Rand auf 24 Pixel fest.

    ![TextBlock-Ränder](images/xaml-basics/margins.png)

    Ränder bieten die einfachste Positionierung eines Elements auf der Seite. Sie sind nützlich für die Feinabstimmung deines Layouts. Du solltest jedoch vermeiden, große Randwerte wie die von Visual Studio hinzugefügten zu verwenden. Sie erschweren die Anpassung der Benutzeroberfläche an verschiedene Bildschirmgrößen.

    Weitere Informationen hierzu finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md).

8. Klicke in der Dokumentgliederung mit der rechten Maustaste auf **TitleTextBlock**, und wähle dann **Stil bearbeiten** > **Ressource anwenden** > **TitleTextBlockStyle** aus. Mit diesem Schritt wird ein vom System definierter Stil auf den Titeltext angewendet.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. Gib im Bereich **Eigenschaften** in das Suchfeld **textwrapping** ein, um die **TextWrapping**-Eigenschaft einfach aufzufinden. Wähle den _Eigenschaftenmarker_ für die **TextWrapping**-Eigenschaft aus, um das zugehörige Menü zu öffnen. (Der Eigenschaftenmarker ist das kleine rechteckige Symbol rechts neben jedem Eigenschaftswert. Der Eigenschaftenmarker ist schwarz, um anzugeben, dass die Eigenschaft auf einen nicht standardmäßigen Wert festgelegt ist.) Wähle im Menü **Eigenschaft** die Option **Zurücksetzen** aus, um die **TextWrapping**-Eigenschaft zurückzusetzen.

    Visual Studio fügt diese Eigenschaft hinzu, aber sie ist bereits in dem Stil festgelegt, den Sie angewendet haben, und daher ist sie hier überflüssig.

Du hast deiner App den ersten Teil der Benutzeroberfläche hinzugefügt. Führen Sie die App jetzt aus, um herauszufinden, wie sie aussieht.

Möglicherweise hast du dich gefragt, warum deine App im XAML-Designer mit weißem Text auf schwarzem Hintergrund angezeigt wird. Als du die App ausgeführt hast, wurde schwarzer Text auf einem weißen Hintergrund angezeigt. Der Grund dafür ist, dass Windows über ein dunkles und ein helles Design verfügt, und das Standarddesign je nach Gerät variiert. Auf einem PC ist das Standarddesign hell. Damit die App im XAML-Designer genauso wie auf deinem PC angezeigt wird, kannst du auf das Zahnradsymbol oben im XAML-Designer klicken, um die Geräteeinstellungen für die Vorschau zu öffnen und das Design auf die helle Einstellung zu setzen.

> [!NOTE]
> In diesem Teil des Tutorials hast du ein Steuerelement durch Ziehen und Ablegen hinzugefügt. Sie können ebenfalls ein Steuerelement hinzufügen, indem Sie auf die Toolbox doppelklicken. Probieren Sie es aus, und sehen Sie die Unterschiede im XAML-Code, den Visual Studio generiert.

## <a name="part-2-add-a-gridview-control-by-using-the-xaml-editor"></a>Teil 2: Hinzufügen eines GridView-Steuerelements mit dem XAML-Editor

In Teil 1 hast du einen Einblick in die Verwendung von XAML-Designer und einiger anderer Tools von Visual Studio erhalten. Hier verwenden Sie den XAML-Editor, um direkt mit XAML-Markup zu arbeiten. Wenn Sie mit XAML vertraut sind, halten Sie diese Arbeitsweise möglicherweise für effizienter.

Ersetze zuerst das Stammlayout-[Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) mit [RelativePanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). **RelativePanel** erleichtert die Neuanordnung der Benutzeroberfläche relativ zum Bereich oder anderen Teilen der Benutzeroberfläche. Den Vorteil siehst du im Tutorial [Adaptives XAML-Layout](xaml-basics-adaptive-layout.md). 

Fügen Sie dann ein [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview)-Steuerelement zum Anzeigen Ihrer Daten hinzu.

So fügst du ein Steuerelement mit dem XAML-Editor hinzu:

1. Ändere im XAML-Editor das Stamm-**Grid** in **RelativePanel**.

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

    Weitere Informationen zum Layout mit **RelativePanel** findest du unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Füge unterhalb des **TextBlock**-Elements ein **GridView**-Steuerelement mit dem Namen **ImageGridView** hinzu. Legen Sie die _angefügten Eigenschaften_ des **RelativePanel** fest, um das Steuerelement unter dem Titeltext zu platzieren, und geben Sie an, dass es sich über die gesamte Bildschirmbreite erstrecken soll.

    **Diesen XAML-Code hinzufügen**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Nach TextBlock**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView control here. -->

    </RelativePanel>
    ```

    Weitere Informationen zu angefügten Paneleigenschaften findest du unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Damit das **GridView**-Steuerelement Elemente anzeigt, musst du ihm eine Sammlung von Daten hinzufügen, die angezeigt werden können. Öffnen Sie „MainPage.xaml.cs“, und suchen Sie die **GetItemsAsync**-Methode. Diese Methode füllt eine Sammlung mit dem Namen **Images** auf, eine Eigenschaft, die wir unter **MainPage** hinzugefügt haben.

    Fügen Sie nach der **foreach**-Schleife in **GetItemsAsync** folgende Codezeile hinzu.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    In diesem Schritt wird die **ItemsSource**-Eigenschaft des **GridView**-Steuerelements auf die Sammlung **Images** der App festgelegt. Außerdem werden Elemente bereitgestellt, die das **GridView**-Steuerelement anzeigen kann.

Dies ist ein guter Ausgangspunkt zum Ausführen der App. Stellen Sie sicher, dass alles funktioniert. Sie sieht etwa wie folgt aus.

![App-UI-Prüfpunkt 1](images/xaml-basics/layout-0.png)

Sie werden feststellen, dass in der App noch keine Bilder angezeigt werden. Standardmäßig wird der **ToString**-Wert des Datentyps, der sich in der Sammlung befindet, angezeigt. Als Nächstes erstellen Sie eine Datenvorlage, die definiert, wie die Daten angezeigt werden.

> [!NOTE]
> Weitere Informationen zu Layouts mit **RelativePanel** findest du im Artikel [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Informiere dich, und experimentiere dann mit einigen anderen Layouts, indem du die angefügten **RelativePanel**-Eigenschaften auf **TextBlock** und **GridView** festlegst.

## <a name="part-3-add-a-datatemplate-object-to-display-your-data"></a>Teil 3: Hinzufügen eines DataTemplate-Objekts zum Anzeigen deiner Daten

Erstelle jetzt ein [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)-Objekt, das dem **GridView**-Steuerelement mitteilt, wie deine Daten angezeigt werden sollen. Eine ausführliche Erläuterung zu Datenvorlagen finden Sie unter [Elementcontainer und Vorlagen](../controls-and-patterns/item-containers-templates.md).

Zunächst musst du nur Platzhalter hinzufügen, die dir beim Erstellen des gewünschten Layouts helfen. Im Tutorial [XAML-Datenbindung](../../data-binding/xaml-basics-data-binding.md) kannst du diese Platzhalter durch echte Daten aus der **ImageFileInfo**-Klasse ersetzen. Sie können jetzt die ImageFileInfo.cs-Datei öffnen, wenn Sie überprüfen möchten, wie das Datenobjekt aussieht.

So fügst du eine Datenvorlage einer Rasteransicht hinzu:

1. Öffnen Sie „MainPage.xaml“.

2. Wenn du die Bewertung anzeigen möchtest, verwende das **RadRating**-Steuerelement aus dem [Telerik UI für UWP](https://github.com/telerik/UI-For-UWP)-NuGet-Paket. Fügen Sie einen XAML-Namespace-Verweis hinzu, der den Namespace für das Telerik-Steuerelemente angibt. Füge ihn dem Starttag **Seite** hinzu, direkt nach den anderen `xmlns:`-Einträgen.

    **Diesen XAML-Code hinzufügen**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Nach dem letzten `xmlns:`-Eintrag**

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

3. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wähle im Kontextmenü die Option **Weitere Vorlagen bearbeiten** > **Generierten Inhalt bearbeiten (ItemTemplate)**  > **Leere ... erstellen** aus. Das Dialogfeld **Ressource erstellen** wird geöffnet.

4. Ändere im Dialogfeld den Wert von **Name (Schlüssel)** in **ImageGridView_DefaultItemTemplate**, und wähle dann **OK** aus.

    Folgendes geschieht, wenn du **OK** auswählst:

    - Dem Abschnitt `Page.Resources` von „MainPage.xaml“ wird ein **DataTemplate**-Objekt hinzugefügt.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - Der Dokumentgliederungsbereich wird auf dieses **DataTemplate**-Objekt festgelegt.

        Wenn du die Datenvorlage erstellt hast, kannst du den Pfeil in der oberen linken Ecke der Dokumentgliederung auswählen, um zum Seitenbereich zurückzukehren.

    - Die **ItemTemplate**-Eigenschaft von **GridView** wird auf die **DataTemplate**-Ressource festgelegt.

       ```xaml
           <GridView x:Name="ImageGridView"
                     Margin="0,0,0,8"
                     RelativePanel.AlignLeftWithPanel="True"
                     RelativePanel.AlignRightWithPanel="True"
                     RelativePanel.Below="TitleTextBlock"
                     ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
       ```

5. Weise in der **ImageGridView_DefaultItemTemplate**-Ressource dem Stamm-**Grid** eine Höhe und Breite von **300** sowie einen Rand von **8** zu. Füge dann zwei Zeilen hinzu, und lege die Höhe der zweiten Zeile auf **Auto** fest.

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

    Weitere Informationen zu **Grid**-Layouts findest du unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid).

6. Füge dem **Grid**-Layout Steuerelemente hinzu.

    a. Fügen Sie ein **Image**-Steuerelement in die erste Rasterzeile ein. An dieser Stelle wird das Bild angezeigt. Doch im Moment kannst du das Logo des Stores der App als Platzhalter verwenden.

    b. Fügen Sie **TextBlock**-Steuerelemente zum Anzeigen des Namens, Dateityps und der Dimensionen des Bilds hinzu. Verwenden Sie hierzu **StackPanel**-Steuerelemente, um die Textblöcke anzuordnen.

    Weitere Informationen zu **StackPanel**-Layouts findest du unter [Layoutpanels](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)

    c. Füge das **RadRating**-Steuerelement dem äußeren (vertikalen) **StackPanel** hinzu. Platziere es nach dem inneren (horizontalen) **StackPanel**.

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

Führe jetzt die App aus, um das **GridView**-Steuerelement mit der soeben erstellten Elementvorlage anzuzeigen. Möglicherweise wird das Bewertungssteuerelement nicht angezeigt, da es weiße Sterne auf weißem Hintergrund aufweist. Im nächsten Schritt ändern Sie die Hintergrundfarbe.

![App-UI-Prüfpunkt 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Teil 4: Ändern des Elementcontainerstils

Eine Steuerelementvorlage für ein Element enthält die optischen Elemente zur Anzeige des Zustands wie Auswahl, Draufzeigen und Fokus. Diese visuellen Elemente werden über oder unter der Datenvorlage gerendert. Hier ändern Sie die **Background**- und **Margin**-Eigenschaften der Steuerelementvorlage, um dem **GridView**-Elemente einen grauen Hintergrund hinzuzufügen.

So änderst du den Elementcontainer:

1. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wähle im Kontextmenü **Weitere Vorlagen bearbeiten** > **Erzeugten Elementcontainer bearbeiten (ItemContainerStyle)**  > **Kopie bearbeiten** aus. Das Dialogfeld **Ressource erstellen** wird geöffnet.

2. Ändere im Dialogfeld den Wert von **Name (Schlüssel)** in **ImageGridView_DefaultItemContainerStyle**, und wähle dann **OK** aus.

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

    Der **GridViewItem**-Standardstil legt zahlreiche Eigenschaften fest. Du solltest immer mit einer Kopie des Standardstils beginnen und nur die notwendigen Eigenschaften ändern. Andernfalls werden die visuellen Elemente wahrscheinlich nicht so angezeigt, wie Sie es erwarten, da einige Eigenschaften nicht richtig festgelegt sind.

    Wie im vorherigen Schritt wird die **ItemContainerStyle**-Eigenschaft des **GridView**-Steuerelements auf die neue **Stil**-Ressource festgelegt.

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

Führen Sie die App aus, um herauszufinden, wie sie nun aussieht. Ändern Sie die Größe des App-Fensters. Das **GridView**-Steuerelement kümmert sich um das Neuanordnen der Bilder; bei einigen Breiten ist jedoch noch viel Platz auf der rechten Seite des App-Fensters vorhanden. Es würde besser aussehen, wenn die Bilder zentriert werden. Damit werden wir uns als Nächstes befassen.

![App-UI-Prüfpunkt 3](images/xaml-basics/layout-2.png)

> [!Note]
> Wenn du experimentieren möchtest, versuche, für die **Background**- und **Margin**-Eigenschaft unterschiedliche Werten festzulegen, und stelle fest, welche Auswirkung dies hat.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Teil 5: Anwenden einiger abschließender Anpassungen am Layout

Um die Bilder auf der Seite zu zentrieren, musst du die Ausrichtung des **Grid**-Steuerelements auf der Seite anpassen. Oder möchtest du die Ausrichtung der Bilder in **GridView** anpassen? Ist es wichtig? Mal sehen.

Weitere Informationen zur Ausrichtung finden Sie unter [Ausrichtung, Ränder und Abstand](../layout/alignment-margin-padding.md).

(Du kannst in diesem Schritt versuchen, die Einstellung der **Background**-Eigenschaft von **GridView** auf deine bevorzugte Farbe festzulegen. So sehen Sie deutlicher, was mit dem Layout passiert.)

So änderst du die Ausrichtung der Bilder:

1. Lege in **GridView** die **HorizontalAlignment**-Eigenschaft auf **Center** fest.

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

    Die Bilder werden zentriert, was besser aussieht. Die Scrollleiste wird jedoch am Rand des **GridView**-Steuerelements und nicht am Rand des Fensters ausgerichtet. Um dieses Problem zu beheben, zentriere die Bilder in **GridView**, anstatt **GridView** auf der Seite zu zentrieren. Es ist etwas mehr Arbeit, sieht aber am Ende besser aus.

3. Entfernen Sie die **HorizontalAlignment**-Einstellung aus dem vorherigen Schritt.

4. Klicken Sie mit der rechten Maustaste unter Dokumentgliederung auf **ImageGridView**. Wähle im Kontextmenü **Weitere Vorlagen bearbeiten** > **Layout der Elemente bearbeiten (ItemsPanel)**  > **Kopie bearbeiten** aus. Das Dialogfeld **Ressource erstellen** wird geöffnet.

5. Ändere im Dialogfeld den Wert von **Name (Schlüssel)** in **ImageGridView_ItemsPanelTemplate**, und wähle dann **OK** aus.

    Eine Kopie der Standard-**ItemsPanelTemplate** wird dem Abschnitt **Page.Resources** im XAML-Code hinzugefügt. (Wie zuvor wird **GridView** aktualisiert, um auf diese Ressource zu verweisen.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    Genau wie die verschiedenen Bereiche für das Layout der Steuerelemente in deiner App verwendet wurden, enthält **GridView** einen internen Bereich, in dem das Layout der Elemente verwaltet wird. Da du nun Zugriff auf diesen Bereich hast (**ItemsWrapGrid**), kannst du seine Eigenschaften ändern, um das Layout der Elemente im **GridView**-Steuerelement zu ändern.

6. Lege in **ItemsWrapGrid** die **HorizontalAlignment**-Eigenschaft auf **Center** fest.

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

Nun ist die Scrollleiste am Rand des Fensters ausgerichtet. Gut gemacht! Sie haben die grundlegende Benutzeroberfläche für Ihre App erstellt.

## <a name="go-further"></a>Fortfahren

Nachdem du nun die grundlegende Benutzeroberfläche erstellt hast, schau dir diese weiteren Tutorials an. Sie basieren auch auf dem PhotoLab-Beispiel. 

* Fügen Sie echte Bilder und Daten im [XAML-Datenbindungs-Tutorial](../../data-binding/xaml-basics-data-binding.md) hinzu.
* Passen Sie die Benutzeroberfläche im [XAML-Layout-Tutorial](xaml-basics-adaptive-layout.md) an unterschiedliche Bildschirmgrößen an.


## <a name="get-the-final-version-of-the-photolab-sample"></a>Abrufen der endgültigen Version des PhotoLab-Beispiels

Dieses Tutorial behandelt nicht die gesamte Fotobearbeitungs-App. Lies daher die [endgültige Version](https://github.com/Microsoft/Windows-appsample-photo-lab), um Features wie z.B. benutzerdefinierte Animationen und Telefonsupport kennenzulernen.

