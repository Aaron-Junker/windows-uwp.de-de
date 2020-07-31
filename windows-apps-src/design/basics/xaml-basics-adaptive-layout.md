---
title: 'Tutorial: Erstellen von adaptiven Layouts'
description: In diesem Artikel werden die Grundlagen für adaptives Layout in XAML behandelt
keywords: XAML, UWP, Erste Schritte
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 389b4ec2566a392de35ae43505274511b4294f9f
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997787"
---
# <a name="tutorial-create-adaptive-layouts"></a>Tutorial: Erstellen von adaptiven Layouts

In diesem Lernprogramm werden die Grundlagen der Verwendung der adaptiven und maßgeschneiderten Layoutfeatures von XAML beschrieben, mit denen Sie Apps erstellen können, die auf jedem Gerät exzellent aussehen. Sie erfahren, wie Sie ein neues DataTemplate erstellen, Fenster-Andockpunkte hinzufügen und das Layout Ihrer App mit den Elementen von VisualStateManager und AdaptiveTrigger anpassen. Wir verwenden diese Tools zum Optimieren der Bildbearbeitungs-Beispiel-App für kleinere Gerätebildschirme. 

Das Bildbearbeitungsprogramm, an dem Sie arbeiten werden, hat zwei Seiten/Bildschirme:

Die **Hauptseite** zeigt eine Ansicht der Foto-Galerie, zusammen mit einigen Informationen über jede Bilddatei an.

![MainPage](../basics/images/xaml-basics/mainpage.png)

Die **Detailseite** zeigt ein einzelnes Foto an, nachdem es ausgewählt wurde. Über ein Flyout-Menü kann das Foto bearbeitet, umbenannt und gespeichert werden.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio 2019: [Visual Studio 2019 Community herunterladen (kostenlos)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK (10.0.15063.468 oder höher):  [Das aktuelle Windows SDK herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Emulator für Windows Mobile: [Windows 10 Mobile-Emulator herunterladen (kostenlos)](https://developer.microsoft.com/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>Teil 0: Startcode von Github holen

Für dieses Tutorial starten Sie mit einer vereinfachten Version des PhotoLab-Beispiels. 

1. Wechseln Sie zu [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Sie gelangen auf die GitHub-Seite für das Beispiel. 
2. Als Nächstes müssen Sie das Beispiel klonen oder herunterladen. Klicken Sie auf die Schaltfläche **Clone or download**. Ein Untermenü erscheint.
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Das Menü <b>Clone or download</b> auf der GitHub-Seite des Fotobeispiels.</figcaption>
    </figure>

    **Wenn Sie mit GitHub nicht vertraut sind:**
    
    ein. Klicken Sie auf **Download ZIP**, und speichern Sie die Datei lokal. Es wird eine ZIP-Datei heruntergeladen, die alle benötigten Projektdateien enthält.

    b. Entpacken Sie die Datei. Verwenden Sie den Datei-Explorer, um zu der gerade heruntergeladenen ZIP-Datei zu navigieren, klicken Sie mit der rechten Maustaste darauf und wählen Sie **Alle extrahieren...** aus. 

    c. Navigieren Sie zu Ihrer lokalen Kopie des Beispiels, und wechseln Sie zum Verzeichnis `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.    

    **Wenn Sie mit GitHub vertraut sind:**

    ein. Klonen Sie den Master-Branch des Repositorys lokal.

    b. Wechseln Sie zum `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`-Verzeichnis.

3. Öffnen Sie das Projekt durch einen Klick auf `Photolab.sln`.

## <a name="part-1-run-the-mobile-emulator"></a>Teil 1: Ausführen des Mobile-Emulators

Stellen Sie sicher, dass Ihre Lösungsplattform in der Visual Studio-Symbolleiste auf x86 oder x64 und nicht ARM festgelegt ist, und ändern Sie Ihr Zielgerät vom lokalen Computer auf einen der Mobile-Emulatoren, die Sie installiert haben (z. B. Mobile Emulator 10.0.15063 WVGA 5 Zoll 1 GB). Führen Sie die Fotogalerie-App im ausgewählten Mobile Emulator durch Drücken von **F5** aus.

Sobald die App gestartet wird, sehen Sie wahrscheinlich, dass die App zwar funktioniert, sie allerdings auf einem kleinen Viewport nicht so gut aussieht. Das dynamische Grid-Element passt sich der begrenzten Bildschirmfläche so gut wie möglich an, indem es die Anzahl der angezeigten Spalten reduziert. Dies erzeugt ein einfallsloses Layout, das unangebracht für diesen kleinen Viewport aussieht.

![Mobiles Layout: danach](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>Teil 2: Erstellen eines mobilen maßgeschneiderten Layouts
Damit diese App auch auf kleineren Geräten gut aussieht, werden wir eine Reihe von Stilen auf unserer XAML-Seite erstellen, die nur dann angewandt werden, wenn ein mobiles Gerät erkannt wird.

### <a name="create-a-new-datatemplate"></a>Erstellen einer neuen DataTemplate
Wir werden die Fotogalerie-Ansicht der Anwendung anpassen, indem wir eine neue Datenvorlage für die Bilder erstellen. Öffnen Sie „MainPage.xaml“ im Projektmappen-Explorer, und fügen Sie den **Page.Resources**-Tags folgenden Code hinzu.

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Diese Galerievorlage spart durch den Wegfall des Bilderrahmens Platz auf dem Bildschirm und beseitigt die Bildmetadaten (Dateiname, Bewertungen usw.) unter jeder Miniaturansicht. Stattdessen wird jede Miniaturansicht als ein einfaches Quadrat angezeigt.

### <a name="add-metadata-to-a-tooltip"></a>Einer QuickInfo Metadaten hinzufügen
Wir möchten dennoch, dass der Benutzer Zugriff auf die Metadaten für jedes Bild hat, daher fügen wir wir jedem Bildelement eine QuickInfo hinzu. Fügen Sie folgenden Code in die **Image**-Tags für die Datenvorlage hinzu, die Sie gerade erstellt haben.

```XAML
<Image ...>

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
</Image>
```

Dadurch werden Titel, Dateityp und Dimensionen eines Bilds angezeigt, wenn Sie mit dem Mauszeiger über die Miniaturansicht zeigen (oder sie auf dem Touchscreen gedrückt halten).

### <a name="add-a-visualstatemanager-and-statetrigger"></a>Hinzufügen eines VisualStateManager und StateTrigger

Wir haben jetzt ein neues Layout für die Daten erstellt, aber die App weiß derzeit nicht, wann dieses Layout anstelle der Standardstile verwendet werden soll. Um dieses Problem zu beheben, müssen wir einen **VisualStateManager** hinzufügen. Fügen Sie dem Stammelement der Seite **RelativePanel** folgenden Code hinzu.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Dadurch wird ein neuer **VisualState** und **StateTrigger** hinzugefügt, der ausgelöst wird, wenn die App erkennt, dass sie auf einem mobilen Gerät ausgeführt wird (die Logik für diesen Vorgang finden Sie in „MobileScreenTrigger.cs“. Dies wird für Sie im PhotoLab-Verzeichnis bereitgestellt). Wenn **StateTrigger** gestartet wird, verwendet die App alle Layoutattribute, die diesem **VisualState** zugewiesen sind.

### <a name="add-visualstate-setters"></a>Hinzufügen von VisualState-Setter
Als Nächstes verwenden wir **VisualState**-Setter, um den **VisualStateManager** anzuweisen, welche bestimmten Attribute angewendet werden sollen, wenn der Zustand ausgelöst wird. Jeder Setter ist auf eine Eigenschaft eines bestimmten XAML-Elements ausgerichtet und legt sie auf den angegebenen Wert fest. Fügen Sie diesen Code dem mobilen **VisualState** hinzu, den Sie gerade erstellt haben, und zwar unter dem **VisualState.StateTriggers**‑Element. 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

Diese Setter legen das **ItemTemplate** der Bildergalerie auf das neue **DataTemplate** fest, dass wir im ersten Teil erstellt haben und richtet die Befehlsleiste und den Zoom-Schieberegler am unteren Rand des Bildschirms aus, damit sie auf einem Mobiltelefon einfacher mit dem Daumen erreichbar sind.

### <a name="run-the-app"></a>Ausführen der App
Versuchen Sie nun, die App mit einem Mobile-Emulator auszuführen. Wird das neue Layout erfolgreich angezeigt? Ein Raster mit Miniaturansichten sollte wie folgt angezeigt werden. Wenn das alte Layout weiterhin angezeigt wird, liegt möglicherweise ein Schreibfehler in Ihrem **VisualStateManager**-Code vor.

![Mobiles Layout: danach](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>Teil 3: Anpassen an mehrere Fenstergrößen auf einem einzigen Gerät
Das Erstellen eines neuen maßgeschneiderten Layouts löst die Herausforderung eines dynamischen Designs für Mobilgeräte, aber was geschieht bei Desktops und Tablet-PCs? Die App kann im Vollbildmodus ansprechend aussehen, aber wenn der Benutzer das Fenster verkleinert, kann sie unter Umständen komisch wirken. Wir garantieren, dass die Oberfläche für Endbenutzer immer gut aussieht, wenn Sie **VisualStateManager** an mehrere Fenstergrößen auf einem einzigen Gerät anpassen.

![Kleines Fenster: vorher](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>Hinzufügen von Fenster-Andockpunkten
Definieren Sie zuerst die „Andockpunkte“, an denen verschiedene **VisualStates** ausgelöst werden sollen. Öffnen Sie App.xaml im Projektmappen-Explorer, und fügen Sie den **Application**-Tags folgenden Code hinzu.

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

Dadurch erhalten wir drei Andockpunkte, die das Erstellen neuer **VisualStates** für drei Fenstergrößen ermöglichen:
+ Klein (0 – 640 Pixel breit)
+ Mittel (641 –1.007 Pixel breit)
+ Groß (> 1.007 Pixel breit)

### <a name="create-new-visualstates-and-statetriggers"></a>Erstellen neuer VisualStates und StateTriggers
Als Nächstes erstellen wir **VisualStates** und **StateTriggers**, die den einzelnen Andockpunkten entsprechen. Fügen Sie dem in Teil 2 erstellten **VisualStateManager** in „MainPage.xaml.cpp“ den folgenden Code hinzu.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>Hinzufügen von Settern
Fügen Sie diese Setter schließlich dem **SmallWindow** hinzu.

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

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

</VisualState>

```

Diese Setter wenden das mobile **DataTemplate** und Stile auf die Desktop-App an, wenn der Viewport kleiner als 641 Pixel breit ist. Sie passen auch den Zoomschieberegler besser an den kleinen Bildschirm an.

### <a name="run-the-app"></a>Ausführen der App

Legen Sie in der Visual Studio-Symbolleiste das Zielgerät auf **Lokaler Computer** fest, und führen Sie die App aus. Wenn die App geladen wird, versuchen Sie, die Größe des Fensters zu ändern. Wenn Sie das Fenster auf eine kleinere Größe anpassen möchten, sollten Sie die App auf das mobile Layout wechseln, das Sie in Teil 2 erstellt haben.

![Kleines Fenster: nachher](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Vertiefung

Nach Abschluss dieser Übung verfügen Sie über ausreichende Kenntnisse über das adaptive Layout, um selbst weiter zu experimentieren. Versuchen Sie, ein Bewertungs-Steuerelement für die mobilen QuickInfos hinzuzufügen, die Sie zuvor erstellt haben. Oder wenn Sie eine größere Herausforderung möchten, optimieren Sie das Layout für größere Bildschirme (wie TV-Bildschirme oder ein Surface Studio)

Wenn Sie Probleme haben, finden Sie weitere Unterstützung in den folgenden Abschnitten von [Definieren von Seitenlayouts mit XAML](../layout/layouts-with-xaml.md).

+ [Visuelle Zustände und Zustandsauslöser](https://docs.microsoft.com/windows/uwp/design/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [Maßgeschneiderte Layouts](https://docs.microsoft.com/windows/uwp/design/layout/layouts-with-xaml#tailored-layouts)

Auch wenn Sie weitere Informationen erhalten möchten, wie die erste Fotobearbeitungs-App erstellt wurde, sehen Sie sich diese Lernprogramme zu [Benutzeroberflächen](../basics/xaml-basics-ui.md) und [Datenbindung](../../data-binding/xaml-basics-data-binding.md) in XAML an.

## <a name="get-the-final-version-of-the-photolab-sample"></a>Abrufen der endgültigen Version des PhotoLab-Beispiels

In diesem Lernprogramm wird nicht das Erstellen der kompletten Fotobearbeitungs-App erläutert. Lesen Sie daher die [endgültige Version](https://github.com/Microsoft/Windows-appsample-photo-lab) zu weiteren Features wie z. B. benutzerdefinierte Animationen und Stile.
