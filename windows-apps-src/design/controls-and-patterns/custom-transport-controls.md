---
description: Der MediaPlayer verfügt über anpassbare XAML-Transportsteuerelemente, um die Steuerung von Audio-und Videoinhalten zu verwalten.
title: Erstellen benutzerdefinierter Medientransportsteuerelemente
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3329ea23c831b1bd91955013f0bdedcbbd08aa34
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032933"
---
# <a name="create-custom-transport-controls"></a>Erstellen benutzerdefinierter Transportsteuerelemente



MediaPlayerElement verfügt über anpassbare XAML-Transportsteuerelemente, um die Steuerung von Audio-und Videoinhalten innerhalb einer Windows-App zu verwalten. Hier wird gezeigt, wie Sie die MediaTransportControls-Vorlage anpassen. Wir zeigen Ihnen, wie Sie das Überlaufmenü verwenden, eine benutzerdefinierte Schaltfläche hinzufügen und den Schieberegler bearbeiten können.

> **Wichtige APIs:** [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement), [MediaPlayerElement.AreTransportControlsEnabled](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled), [MediaTransportControls](/uwp/api/Windows.Media.SystemMediaTransportControls)

Bevor Sie beginnen, sollten Sie mit den MediaPlayerElement- und MediaTransportControls-Klassen vertraut sein. Weitere Informationen finden Sie im Leitfaden für das MediaPlayerElement-Steuerelement.

> [!TIP]
> Die Beispiele in diesem Thema basieren auf dem [Beispiel für die Steuerelemente für den Medientransport](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls). Sie können das Beispiel herunterladen, um den fertigen Code anzuzeigen und auszuführen.

> [!NOTE]
> **MediaPlayerElement** steht erst ab Windows 10, Version 1607, zur Verfügung. Für das Entwickeln von Apps für niedrigere Windows 10-Versionen muss stattdessen [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) verwendet werden. Alle Beispiele auf dieser Seite funktionieren auch mit **MediaElement**.

## <a name="when-should-you-customize-the-template"></a>Wann sollte die Vorlage angepasst werden?

**MediaPlayerElement** verfügt über integrierte Transportsteuerelemente, die so konzipiert sind, dass sie standardmäßig mit den meisten Apps für die Video- und Audiowiedergabe verwendet werden können. Sie werden von der [**MediaTransportControls**](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls)-Klasse bereitgestellt und enthalten Schaltflächen zum Wiedergeben und Beenden von sowie zum Navigieren in Medien, zum Einstellen der Lautstärke, zum Aktivieren/Deaktivieren des Vollbildmodus, zum Übertragen auf ein Zweitgerät, zum Aktivieren von Untertiteln, zum Wechseln von Audiospuren und zum Anpassen der Wiedergaberate. MediaTransportControls verfügt über Eigenschaften, mit denen Sie steuern können, ob die einzelnen Schaltflächen angezeigt und aktiviert werden. Sie können auch die [**IsCompact**](/uwp/api/windows.ui.xaml.controls.mediatransportcontrols.iscompact)-Eigenschaft festlegen, um anzugeben, ob die Steuerelemente in einer Zeile oder in zwei Zeilen angezeigt werden.

Allerdings gibt es möglicherweise Szenarien, in denen Sie das Erscheinungsbild des Steuerelements weiter anpassen oder sein Verhalten ändern müssen. Dies sind einige Beispiele:
- Ändern Sie die Symbole, das Schiebereglerverhalten und die Farben.
- Verschieben Sie weniger häufig verwendete Befehlsschaltflächen in ein Überlaufmenü.
- Ändern Sie die Reihenfolge, in der Befehle wegfallen, wenn die Größe des Steuerelements geändert wird.
- Stellen Sie eine Befehlsschaltfläche bereit, die nicht im Standardsatz enthalten ist.

> [!NOTE]
> Wenn auf dem Bildschirm nicht genügend Platz vorhanden ist, fallen die angezeigten Schaltflächen in einer vordefinierten Reihenfolge aus den integrierten Transportsteuerelementen weg. Um diese Reihenfolge zu ändern, oder um Befehle zu platzieren, die nicht in ein Überlaufmenü passen, müssen Sie die Steuerelemente anpassen.

Sie können die Darstellung des Steuerelements durch Ändern der Standardvorlage anpassen. Wenn Sie das Verhalten des Steuerelements ändern oder neue Befehle hinzufügen möchten, können Sie ein benutzerdefiniertes Steuerelement erstellen, das von MediaTransportControls abgeleitet wird.

> [!TIP]
> Anpassbare Steuerelementvorlagen sind ein leistungsfähiges Feature der XAML-Plattform, aber bei ihrer Nutzung ergeben sich auch Konsequenzen, die Sie berücksichtigen sollten. Wenn Sie eine Vorlage anpassen, wird sie zu einem statischen Teil Ihrer App und erhält daher keine Plattformupdates, die von Microsoft für die Vorlage durchgeführt werden. Wenn von Microsoft Vorlagenupdates herausgegeben werden, sollten Sie die neue Vorlage nutzen und erneut anpassen, um von den Vorteilen der aktualisierten Vorlage zu profitieren.

## <a name="template-structure"></a>Vorlagenstruktur

Das [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Element ist im Standardstil enthalten. Sie können diesen Standardstil in Ihr Projekt kopieren, um ihn zu ändern. Das ControlTemplate-Element ist in Abschnitte unterteilt, ähnlich wie andere XAML-Steuerelementvorlagen.
- Der erste Abschnitt der Vorlage enthält die [**Style**](/uwp/api/Windows.UI.Xaml.Style)-Definitionen für die verschiedenen Komponenten von MediaTransportControls.
- Im zweiten Abschnitt werden die verschiedenen visuellen Zustände definiert, die von MediaTransportControls verwendet werden.
- Der dritte Abschnitt enthält das [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)-Element, in dem die verschiedenen MediaTransportControls-Elemente zusammengeführt werden und mit dem das Layout der Komponenten definiert wird.

> [!NOTE]
> Weitere Informationen zum Ändern von Vorlagen finden Sie unter [Steuerelementvorlagen](./control-templates.md). Sie können einen Text-Editor oder vergleichbare Editoren in Ihrer IDE verwenden, um die XAML-Dateien in „\(*Programmdateien* )\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\( *SDK-Version* )\Generic“ zu öffnen. Das Standardformat und die Vorlage für jedes Steuerelement werden in der Datei **generic.xaml** definiert. Sie finden die MediaTransportControls-Vorlage in generic.xaml, indem Sie nach „MediaTransportControls“ suchen.

In den folgenden Abschnitten erfahren Sie, wie Sie einige der wichtigsten Elemente der Transportsteuerelemente anpassen:
- [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider): Ermöglicht Benutzern das Scrubbing durch ihre Medien und zeigt darüber hinaus den Fortschritt an.
- [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar): Enthält alle Schaltflächen.
Weitere Informationen finden Sie im Abschnitt zum Aufbau des MediaTransportControls-Referenzthemas.

## <a name="customize-the-transport-controls"></a>Anpassen der Transportsteuerelemente

Wenn Sie nur die Darstellung von MediaTransportControls ändern möchten, können Sie einfach eine Kopie des standardmäßigen Steuerelementstils und der Vorlage erstellen und diese ändern. Wenn Sie jedoch auch die Funktionalität des Steuerelements erweitern oder ändern möchten, müssen Sie eine neue von MediaTransportControls abgeleitete Klasse erstellen.

### <a name="re-template-the-control"></a>Anpassen der Vorlage eines Steuerelements

**So passen Sie Standardstil und Standardvorlage von MediaTransportControls an**
1. Kopieren Sie den Standardstil aus den MediaTransportControls-Stilen und-Vorlagen in ein ResourceDictionary in Ihrem Projekt.
2. Weisen Sie dem Style einen x:Key-Wert zu, um ihn zu identifizieren, wie hier gezeigt.

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. Fügen Sie Ihrer Benutzeroberfläche ein MediaPlayerElement mit MediaTransportControls hinzu.
4. Legen Sie die Style-Eigenschaft des MediaTransportControls-Element auf Ihre benutzerdefinierte Style-Ressource fest, wie hier gezeigt.

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Weitere Informationen zum Ändern von Stilen und Vorlagen finden Sie unter [Formatieren von Steuerelementen](./xaml-styles.md) und [Steuerelementvorlagen](./control-templates.md).

### <a name="create-a-derived-control"></a>Erstellen eines abgeleiteten Steuerelements

Wenn Sie die Funktionalität der Transportsteuerelemente erweitern oder ändern möchten, müssen Sie eine neue von MediaTransportControls abgeleitete Klasse erstellen. Eine abgeleitete Klasse namens `CustomMediaTransportControls` wird im [Beispiel für die Steuerelemente für den Medientransport](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls) und den übrigen Beispielen auf dieser Seite gezeigt.

**So erstellen Sie eine neue Klasse, die von MediaTransportControls abgeleitet ist**
1. Fügen Sie Ihrem Projekt eine neue Klassendatei hinzu.
    - Wählen Sie in Visual Studio Projekt > Klasse hinzufügen. Das Dialogfeld Neues Element hinzufügen wird angezeigt.
    - Geben Sie im Dialogfeld „Neues Element hinzufügen“ einen Namen für die Klassendatei ein, und klicken Sie dann auf „Hinzufügen“. (Im Beispiel für die Medientransportsteuerelemente hat die Klasse den Namen `CustomMediaTransportControls`.)
2. Ändern Sie den Klassencode, um die Ableitung von der MediaTransportControls-Klasse auszuführen.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. Kopieren Sie den Standardstil für [**MediaTransportControls**](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) in einen [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) in Ihrem Projekt. Dies sind der Stil und die Vorlage, die Sie ändern.
(Im Beispiel für die Medientransportsteuerelemente wird ein neuer Ordner mit dem Namen „Themes“ erstellt, dem eine ResourceDictionary-Datei mit dem Namen generic.xaml hinzugefügt wird.)
4. Ändern Sie den [**TargetType**](/uwp/api/windows.ui.xaml.style.targettype) des Stils in den Typ des neuen benutzerdefinierten Steuerelements. (Im Beispiel wird der TargetType in `local:CustomMediaTransportControls`geändert.)

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. Legen Sie den [**DefaultStyleKey**](/uwp/api/windows.ui.xaml.controls.control.defaultstylekey) Ihrer benutzerdefinierten Klasse fest. Dies weist die benutzerdefinierte Klasse an, einen Style mit dem TargetType `local:CustomMediaTransportControls` zu verwenden.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. Fügen Sie dem XAML-Markup ein [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) hinzu, und fügen Sie diesem die benutzerdefinierten Transportsteuerelemente hinzu. Beachten Sie, dass die APIs zum Ausblenden, Anzeigen, Deaktivieren und Aktivieren der Standardschaltflächen mit einer angepassten Vorlage weiterhin funktionieren.

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Sie können nun den Steuerelementstil und die -vorlage ändern, um das Erscheinungsbild des benutzerdefiniertes Steuerelements zu aktualisieren, und den Steuerelementcodes ändern, um das Verhalten zu aktualisieren.

### <a name="working-with-the-overflow-menu"></a>Verwenden des Überlaufmenüs

Sie können MediaTransportControls-Befehlsschaltflächen in ein Überlaufmenü verschieben, sodass weniger häufig verwendete Befehle ausgeblendet werden, bis der Benutzer diese benötigt.

In der MediaTransportControls-Vorlage sind die Befehlsschaltflächen in einem [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)-Element enthalten. Das Konzept der Befehlsleiste basiert auf primären und sekundären Befehlen. Die primären Befehle sind die Schaltflächen, die standardmäßig im Steuerelement angezeigt werden und stets sichtbar sind (es sei denn, Sie deaktivieren die Schaltfläche oder blenden sie aus, oder es ist nicht ausreichend Platz vorhanden). Die sekundären Befehle sind in einem Überlaufmenü enthalten, das angezeigt wird, wenn ein Benutzer auf die Schaltfläche mit den Auslassungspunkten (...) klickt. Weitere Informationen finden Sie im Artikel [App-Leisten und Befehlsleisten](app-bars.md).

Um ein Element aus den primären Befehlsleistenbefehlen in das Überlaufmenü zu verschieben, müssen Sie die XAML-Steuerelementvorlage bearbeiten.

**So verschieben Sie einen Befehl in das Überlaufmenü**
1. Suchen Sie in der Steuerelementvorlage das CommandBar-Element namens `MediaControlsCommandBar`.
2. Fügen Sie dem XAML-Code für CommandBar einen [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)-Abschnitt hinzu. Platzieren Sie diesen nach dem schließenden Tag für das [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)-Element.

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. Um das Menü mit Befehlen zu füllen, schneiden Sie den XAML-Code für die gewünschten [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)-Objekte aus dem PrimaryCommands-Element aus, und fügen Sie ihn in das SecondaryCommands-Element ein. In diesem Beispiel wird das `PlaybackRateButton`-Element in das Überlaufmenü verschoben.

4. Fügen Sie der Schaltfläche eine Beschriftung hinzu, und entfernen Sie die Formatierungsinformationen, wie hier gezeigt.
Da das Überlaufmenü aus Textschaltflächen besteht, müssen Sie der Schaltfläche eine Textbeschriftung hinzufügen und außerdem den Stil entfernen, der die Höhe und Breite der Schaltfläche festgelegt. Andernfalls wird sie nicht richtig im Überlaufmenü angezeigt.

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> Außerdem müssen Sie die Schaltfläche sichtbar machen und aktivieren, damit sie im Überlaufmenü verwendet werden kann. In diesem Beispiel ist das PlaybackRateButton-Element nicht im Überlaufmenü sichtbar, es sei denn, die IsPlaybackRateButtonVisible-Eigenschaft ist auf „true“ festgelegt. Es ist nicht aktiviert, es sei denn, die IsPlaybackRateEnabled-Eigenschaft ist auf „true“ festgelegt. Das Festlegen dieser Eigenschaften wird im vorherigen Abschnitt erläutert.

### <a name="adding-a-custom-button"></a>Hinzufügen einer benutzerdefinierten Schaltfläche

Das Anpassen von MediaTransportControls kann beispielsweise erforderlich sein, wenn Sie dem Steuerelement einen benutzerdefinierten Befehl hinzufügen möchten. Unabhängig davon, ob Sie ihn als primären oder sekundären Befehl hinzufügen, sind die Verfahren zum Erstellen der Befehlsschaltfläche und zum Ändern des Verhaltens gleich. Im [Beispiel für die Medientransportsteuerelemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls) wird den primären Befehlen eine Bewertungsschaltfläche hinzugefügt.

**So fügen Sie eine benutzerdefinierte Befehlsschaltfläche hinzu**
1. Erstellen Sie ein AppBarButton-Objekt, und fügen Sie es dem CommandBar-Element in der Steuerelementvorlage hinzu.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

Sie müssen es dem CommandBar-Element an der entsprechenden Position hinzufügen. (Weitere Informationen finden Sie im Abschnitt „Verwenden des Überlaufmenüs“.) Die Platzierung in der Benutzeroberfläche wird durch die Position der Schaltfläche im Markup bestimmt. Wenn diese Schaltfläche als letztes Element in den primären Befehlen angezeigt werden soll, fügen Sie sie am Ende der Liste der primären Befehle hinzu.

Sie können auch das Symbol für die Schaltfläche anpassen. Weitere Informationen finden Sie in der Referenz zu <a href="/uwp/api/Windows.UI.Xaml.Controls.AppBarButton"><b>AppBarButton</b></a>.
    

2. Rufen Sie in der [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)-Überschreibung die Schaltfläche aus der Vorlage ab, und registrieren Sie einen Handler für das dazugehörige [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis. Dieser Code wird in die `CustomMediaTransportControls`-Klasse eingefügt.

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. Fügen Sie dem Click-Ereignishandler Code hinzu, um die Aktion auszuführen, die beim Klicken auf die Schaltfläche ausgelöst wird.
Dies ist der vollständige Code für die Klasse.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**Benutzerdefinierte Medientransport-Steuerelemente mit hinzugefügter „Gefällt mir“-Schaltfläche**
![Benutzerdefiniertes Medientransport-Steuerelement mit zusätzlicher „Gefällt mir“-Schaltfläche](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>Ändern des Schiebereglers

Das „seek“-Steuerelement von MediaTransportControls wird durch ein [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Element bereitgestellt. Eine Möglichkeit zum Anpassen besteht darin, die Granularität des Suchverhaltens zu ändern.

Der standardmäßige Suchschieberegler ist in 100 Segmente unterteilt, sodass das Suchverhalten auf diese Anzahl von Abschnitten begrenzt ist. Sie können die Granularität des Suchschiebereglers ändern, indem Sie das Slider-Element aus der visuellen XAML-Struktur in Ihrem [**MediaOpened**](/uwp/api/windows.media.playback.mediaplayer.mediaopened)-Ereignishandler für [**MediaPlayerElement.MediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) abrufen. In diesem Beispiel wird gezeigt, wie Sie mithilfe von [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) einen Verweis auf das Slider-Element abrufen, dann das Standardschrittintervall des Schiebereglers von 1 % in 0,1 % (1.000 Schritte) ändern, wenn das Medium länger als 120 Minuten ist. Das MediaPlayerElement hat den Namen `MediaPlayerElement1`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>Verwandte Artikel

- [Medienwiedergabe](media-playback.md)
