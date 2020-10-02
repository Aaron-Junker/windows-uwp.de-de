---
title: 'Tutorial: Erstellen von adaptiven Layouts'
description: Erfahren Sie, wie Sie die adaptiven Layoutfeatures in XAML nutzen können, um Apps zu erstellen, die in jeder Fenstergröße gut aussehen.
keywords: XAML, UWP, Erste Schritte
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aabad7a731bd0654468d7b9849d3b9a5bf6bead6
ms.sourcegitcommit: 662fcfdc08b050947e289a57520a2f99fad1a620
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91353730"
---
# <a name="tutorial-create-adaptive-layouts"></a>Tutorial: Erstellen von adaptiven Layouts

Dieses Tutorial behandelt die Grundlagen der Verwendung der adaptiven Layoutfunktionen von XAML, mit denen Sie Apps erstellen können, die in jeder Größe gut aussehen. Sie erfahren, wie Sie Fensterbreakpoints hinzufügen, eine neue DataTemplate erstellen und die VisualStateManager-Klasse verwenden, um das Layout Ihrer App individuell anzupassen. Sie verwenden diese Tools zum Optimieren eines Bildbearbeitungsprogramms für kleinere Fenstergrößen.

Das Bildbearbeitungsprogramm umfasst zwei Seiten. Die _Hauptseite_ zeigt eine Ansicht der Foto-Galerie zusammen mit einigen Informationen über jede Bilddatei an.

![MainPage](../basics/images/xaml-basics/mainpage.png)

Die *Detailseite* zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Voraussetzungen

+ Visual Studio 2019: [Visual Studio 2019 herunterladen](https://visualstudio.microsoft.com/downloads/) (Community-Edition ist kostenlos)
+ Windows 10 SDK (10.0.17763.0 oder höher):  [Das aktuelle Windows SDK herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, Version 1809 oder höher

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Startcode von Github holen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels.

1. Zur GitHub-Seite für das Beispiel navigieren: [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
2. Als Nächstes müssen Sie das Beispiel klonen oder herunterladen. Klicken Sie auf die Schaltfläche **Clone or download**. Ein Untermenü erscheint.
    ![Das Menü „Clone or download“ (Klonen oder Herunterladen) auf der GitHub-Seite des PhotoLab-Beispiels](images/xaml-basics/clone-repo.png)

    **Wenn Sie mit GitHub nicht vertraut sind:**

    ein. Klicken Sie auf **Download ZIP**, und speichern Sie die Datei lokal. Es wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.

    b. Entpacken Sie die Datei. Verwenden Sie den Datei-Explorer, um zu der soeben heruntergeladenen ZIP-Datei zu navigieren, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Alle extrahieren** aus.

    c. Navigieren Sie zu Ihrer lokalen Kopie des Beispiels, und wechseln Sie in das Verzeichnis `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.

    **Wenn Sie mit GitHub vertraut sind:**

    ein. Klonen Sie den Master-Branch des Repositorys lokal.

    b. Wechsle zum `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`-Verzeichnis.

3. Doppelklicken Sie auf `Photolab.sln`, um die Projektmappe in Visual Studio zu öffnen.

## <a name="part-1-define-window-breakpoints"></a>Teil 1: Definieren von Fensterbreakpoints

Führen Sie die App aus. Sie sieht im Vollbildmodus gut aus, aber die Benutzeroberfläche (UI) ist nicht ideal, wenn Sie das Fenster verkleinern. Sie können sicherstellen, dass die Benutzeroberfläche für den Endbenutzer immer richtig aussieht und sich richtig anfühlt, indem Sie die Klasse [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) verwenden, um die Benutzeroberfläche an verschiedene Fenstergrößen anzupassen.

![Kleines Fenster: vorher](../basics/images/xaml-basics/adaptive-layout-small-before.png)

Weitere Informationen zum App-Layout finden Sie im Abschnitt [Layout](../layout/index.md) der Dokumentation.

### <a name="add-window-breakpoints"></a>Hinzufügen von Fensterbreakpoints

Der erste Schritt besteht darin, die _Breakpoints_ zu definieren, an denen verschiedene visuelle Zustände angewendet werden. Weitere Informationen zu den Breakpoints für kleine, mittlere und große Bildschirme finden Sie unter [Bildschirmgrößen und Breakpoints](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

Öffnen Sie „App.xaml“ im Projektmappen-Explorer, und fügen Sie den folgenden Code nach `MergedDictionaries` vor dem schließenden `</ResourceDictionary>`-Tag hinzu.

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

Dadurch werden drei Breakpoints erstellt, mit denen Sie neue visuelle Zustände für drei Bereiche von Fenstergrößen erstellen können:

+ Klein (0 – 640 Pixel breit)
+ Mittel (641 –1.007 Pixel breit)
+ Groß (> 1.007 Pixel breit)

In diesem Beispiel erstellen Sie ein neues Aussehen nur für die kleine Fenstergröße. Die mittlere und große Größe verwenden dasselbe Aussehen.

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>Teil 2: Hinzufügen einer Datenvorlage für kleine Fenstergrößen

Damit diese App auch dann gut aussieht, wenn sie in einem kleinen Fenster angezeigt wird, können Sie eine neue Datenvorlage erstellen, die optimiert, wie die Bilder in der Bildergalerieansicht angezeigt werden, wenn der Benutzer das Fenster verkleinert.

### <a name="create-a-new-datatemplate"></a>Erstellen einer neuen DataTemplate

 Öffnen Sie „MainPage.xaml“ im Projektmappen-Explorer, und fügen Sie den `Page.Resources`-Tags folgenden Code hinzu.

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Diese Galerievorlage spart durch den Wegfall des Bilderrahmens Platz auf dem Bildschirm und beseitigt die Bildmetadaten (Dateiname, Bewertungen usw.) unter jeder Miniaturansicht. Stattdessen wird jede Miniaturansicht als ein einfaches Quadrat angezeigt.

### <a name="add-metadata-to-a-tooltip"></a>Einer QuickInfo Metadaten hinzufügen

Sie möchten dennoch, dass der Benutzer über Zugriff auf die Metadaten für jedes Bild verfügt, daher fügen Sie jedem Bildelement eine QuickInfo hinzu. Fügen Sie folgenden Code in die `Image`-Tags für die Datenvorlage hinzu, die Sie gerade erstellt haben.

```xaml
    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
```

Dadurch werden Titel, Dateityp und Dimensionen eines Bilds angezeigt, wenn Sie mit dem Mauszeiger über die Miniaturansicht zeigen (oder sie auf dem Touchscreen gedrückt halten).

## <a name="part-3-define-visual-states"></a>Teil 3: Definieren von visuellen Zuständen

Sie haben jetzt ein neues Layout für Ihre Daten erstellt, aber die App weiß derzeit nicht, wann dieses Layout anstelle der Standardstile verwendet werden soll. Um dieses Problem zu beheben, müssen Sie [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager)- und [VisualState](/uwp/api/windows.ui.xaml.visualstate)-Definitionen hinzufügen.

### <a name="add-a-visualstatemanager"></a>Hinzufügen eines VisualStateManager

Fügen Sie dem Stammelement der Seite `RelativePanel` folgenden Code hinzu.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>Erstellen von StateTriggers zum Anwenden des visuellen Zustands

Erstellen Sie nun die `StateTriggers`, die den einzelnen „Andockpunkten“ entsprechen. Fügen Sie in „MainPage.xaml“ den folgenden Code zu jedem `VisualState`-Element hinzu.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Wenn die einzelnen visuellen Zustände ausgelöst werden, verwendet die App alle Layoutattribute, die dem aktiven `VisualState` zugewiesen sind.

### <a name="set-properties-for-each-visual-state"></a>Festlegen von Eigenschaften für jeden visuellen Zustand

Legen Sie abschließend die Eigenschaften für jeden visuellen Zustand fest, um `VisualStateManager` zu informieren, welche Attribute beim Auslösen des Zustands angewendet werden sollen. Jeder Setter ist auf eine Eigenschaft eines bestimmten XAML-Elements ausgerichtet und legt sie auf den angegebenen Wert fest. Fügen Sie dem visuellen Zustand `SmallWindow`, den Sie soeben erstellt haben, diesen Code nach den `StateTriggers` hinzu.

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>
```

Diese Setter legen die `ItemTemplate` der Bildergalerie auf die neue `DataTemplate` fest, die Sie im vorherigen Abschnitt erstellt haben. Sie passen auch den Zoomschieberegler besser an den kleinen Bildschirm an.

### <a name="run-the-app"></a>Ausführen der App

Führen Sie die App aus. Wenn die App geladen wird, versuchen Sie, die Größe des Fensters zu ändern. Wenn Sie das Fenster auf eine kleine Größe verkleinern, sollten Sie sehen, wie die Anwendung zu dem kleinen Layout wechselt, das Sie in Teil 2 erstellt haben.

![Kleines Fenster: nachher](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Vertiefung

Nach Abschluss dieser Übung verfügen Sie über ausreichende Kenntnisse über das adaptive Layout, um selbst weiter zu experimentieren. Als größere Herausforderung können Sie versuchen, das Layout für größere Bildschirmgrößen zu optimieren, wie z. B. für Surface Hub. Wenn Sie ein Surface Hub-Layout testen möchten, finden Sie weitere Informationen unter [Testen von Surface Hub-Apps mit Visual Studio](../../debug-test-perf/test-surface-hub-apps-using-visual-studio.md).

Wenn Sie Probleme haben, finden Sie weitere Unterstützung in den folgenden Abschnitten von [Dynamische Layouts mit XAML](../layout/layouts-with-xaml.md).

+ [Visuelle Zustände und Zustandsauslöser](../layout/layouts-with-xaml.md#adaptive-layouts-with-visual-states-and-state-triggers)
+ [Maßgeschneiderte Layouts](../layout/layouts-with-xaml.md#tailored-layouts)

Auch wenn Sie weitere Informationen erhalten möchten, wie die erste Fotobearbeitungs-App erstellt wurde, sehen Sie sich diese Lernprogramme zu [Benutzeroberflächen](../basics/xaml-basics-ui.md) und [Datenbindung](../../data-binding/xaml-basics-data-binding.md) in XAML an.

## <a name="get-the-final-version-of-the-photolab-sample"></a>Abrufen der endgültigen Version des PhotoLab-Beispiels

In diesem Lernprogramm wird nicht das Erstellen der kompletten Fotobearbeitungs-App erläutert. Lesen Sie daher die [endgültige Version](https://github.com/Microsoft/Windows-appsample-photo-lab) zu weiteren Features wie z. B. benutzerdefinierte Animationen und Stile.
