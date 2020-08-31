---
description: Mit verbundenen Animationen können Sie dynamische und ansprechende Navigationsfunktionen erstellen, indem Sie den Übergang eines Elements zwischen zwei verschiedenen Ansichten animieren.
title: Verbundene Animationen
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ff252faf4dd49929ec46c2ceaa02f94011e6b225
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169324"
---
# <a name="connected-animation-for-windows-apps"></a>Verbundene Animation für Windows-apps

Mit verbundenen Animationen können Sie dynamische und ansprechende Navigationsfunktionen erstellen, indem Sie den Übergang eines Elements zwischen zwei verschiedenen Ansichten animieren. Dadurch kann der Benutzer seinen Kontext beibehalten und die Kontinuität zwischen den Ansichten bereitstellen.

In einer verbundenen Animation scheint ein Element während einer Änderung des UI-Inhalts "Fortfahren" zwischen zwei Ansichten zu wechseln, und über den Bildschirm von seiner Position in der Quell Ansicht bis zum Ziel in der neuen Ansicht zu wechseln. Dadurch wird der gemeinsame Inhalt zwischen den Ansichten hervorgehoben, und es wird ein schöner und dynamischer Effekt als Teil eines Übergangs erzeugt.

> **Wichtige APIs**:  [connectedanimation Class](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [connectedanimationservice-Klasse](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die Katalog-App für <strong style="font-weight: semi-bold">XAML</strong> -Steuerelemente installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ConnectedAnimation">die APP zu öffnen und die verbundene Animation in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

In diesem kurzen Video wird eine APP mit einer verbundenen Animation zum Animieren eines Element Bilds verwendet, da es fortgesetzt wird, um Teil des Headers der nächsten Seite zu werden. Der Effekt hilft bei der Beibehaltung des Benutzer Kontexts über den Übergang.

![Verbundene Animation](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Videozusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Verbundene Animation und das fließende Entwurfs System

 Mit dem Fluent Design-System können Sie moderne, klare Benutzeroberflächen erstellen, die Licht, Tiefe, Bewegung, Material und Skalierung enthalten. Connected Animation ist eine fließende Entwurfs System Komponente, die Bewegung zu Ihrer APP hinzufügt. Weitere Informationen finden Sie in der [Übersicht über Fluent Design](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>Warum verbundene Animation?

Beim Navigieren zwischen den Seiten ist es wichtig, dass der Benutzer weiß, welche neuen Inhalte nach der Navigation angezeigt werden und wie Sie sich auf ihre Absicht bezieht, wenn Sie navigieren. Verbundene Animationen stellen eine leistungsstarke visuelle Metapher dar, mit der die Beziehung zwischen zwei Ansichten hervorgehoben wird, indem der Fokus des Benutzers auf den von Ihnen gemeinsam genutzten Inhalt gezeichnet wird. Darüber hinaus ergänzen verbundene Animationen visuelle Interessen und die Navigation in der Seitennavigation, mit deren Hilfe der Bewegungs Entwurf Ihrer APP unterschieden werden kann.

## <a name="when-to-use-connected-animation"></a>Verwendungszwecke der verbundenen Animation

Verbundene Animationen werden im Allgemeinen verwendet, wenn Seiten geändert werden. Sie können jedoch auf jede beliebige Oberfläche angewendet werden, bei der Sie den Inhalt in einer Benutzeroberfläche ändern und den Benutzer den Kontext beibehalten möchten. Sie sollten die Verwendung einer verbundenen Animation anstelle eines Drilldowns [im Navigations Übergang in](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) Erwägung gezogen, wenn ein Bild oder eine andere Benutzeroberfläche von der Quell-und Ziel Ansicht gemeinsam genutzt wird.

## <a name="configure-connected-animation"></a>Verbundene Animation konfigurieren

> [!IMPORTANT]
> Für dieses Feature ist es erforderlich, dass die Zielversion Ihrer APP Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher ist. Die-Konfigurations Eigenschaft ist in früheren sdert nicht verfügbar. Sie können eine minimale Version als SDK 17763 als Ziel verwenden, indem Sie adaptiven Code oder bedingtes XAML verwenden. Weitere Informationen finden Sie unter [Version Adaptive apps](../../debug-test-perf/version-adaptive-apps.md).

Ab Windows 10, Version 1809, können verbundene Animationen den fließenden Entwurf weiter untersuchen, indem Sie Animations Konfigurationen bereitstellen, die speziell für vorwärts-und rückwärts Seitennavigation zugeschnitten sind

Sie geben eine Animations Konfiguration an, indem Sie die Konfigurations Eigenschaft für "connectedanimation" festlegen. (Im nächsten Abschnitt werden Beispiele dafür angezeigt.)

In dieser Tabelle werden die verfügbaren Konfigurationen beschrieben. Weitere Informationen zu den in diesen Animationen angewendeten Bewegungsprinzipien finden Sie unter [Direktionalität und Schwerkraft](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Dies ist die Standardkonfiguration, die für die Vorwärtsnavigation empfohlen wird. |
Wenn der Benutzer in der APP vorwärts navigiert (A zu B), wird das verbundene Element physisch als "Pull Off the page" angezeigt. Dabei scheint das-Element in z-Space vorwärts zu gehen und ein wenig als Auswirkung der Schwerkraft zu haben. Um die Auswirkungen der Schwerkraft zu überwinden, gewinnt das-Element Geschwindigkeit und beschleunigt die endgültige Position. Das Ergebnis ist eine "Skalierung und Dip"-Animation. |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| Wenn der Benutzer in der APP rückwärts navigiert (B zu A), ist die Animation direkter. Das verbundene Element übersetzt linear von B zu a und verwendet dabei eine verlangsamte gezier Ende Beschleunigungs Funktion. Der rückwärts visuelle Vorgang gibt den Benutzer so schnell wie möglich an den vorherigen Zustand zurück, während er weiterhin den Kontext des Navigations Flusses beibehält. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Dies ist die standardmäßige (und einzige) Animation, die in Versionen vor Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)), verwendet wird. |

### <a name="connectedanimationservice-configuration"></a>Connectedanimationservice-Konfiguration

Die [connectedanimationservice](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) -Klasse verfügt über zwei Eigenschaften, die auf die einzelnen Animationen anstatt auf den gesamten Dienst angewendet werden.

- [Defaultduration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [Defaulteasingfunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Um die verschiedenen Auswirkungen zu erzielen, werden diese Eigenschaften von einigen Konfigurationen auf connectedanimationservice ignoriert und stattdessen ihre eigenen Werte verwendet, wie in dieser Tabelle beschrieben.

| Konfiguration | Respektiert defaultduration? | Respektiert defaulteasingfunction? |
| - | - | - |
| Ernst | Ja | Ja* <br/> **Die grundlegende Übersetzung von A in B verwendet diese Beschleunigungs Funktion, aber die "Gravity Dip" hat eine eigene Beschleunigungs Funktion.*  |
| Direkt | Nein <br/> *Animiert mehr als 150 ms.*| Nein <br/> *Verwendet die verlangsamnde Beschleunigungs Funktion.* |
| Basic | Ja | Ja |

## <a name="how-to-implement-connected-animation"></a>Implementieren einer verbundenen Animation

Das Einrichten einer verbundenen Animation umfasst zwei Schritte:

1. *Bereiten* Sie ein Animations Objekt auf der Seite Quelle vor, das dem System anzeigt, dass das Quell Element an der verbundenen Animation beteiligt ist.
1. *Starten* Sie die Animation auf der Zielseite, und übergeben Sie einen Verweis auf das Ziel Element.

Wenn Sie von der Seite "Quelle" aus navigieren, können Sie [connectedanimationservice. getforcurrentview](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) aufrufen, um eine Instanz von connectedanimationservice abzurufen. Um eine Animation vorzubereiten, nennen Sie [preparetoanimieren](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) für diese Instanz, und übergeben Sie einen eindeutigen Schlüssel und das UI-Element, das Sie im Übergang verwenden möchten. Mit dem eindeutigen Schlüssel können Sie die Animation später auf der Zielseite abrufen.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Wenn die Navigation erfolgt, starten Sie die Animation auf der Zielseite. Um die Animation zu starten, wenden Sie [connectedanimation. trystart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)an. Sie können die richtige Animations Instanz abrufen, indem Sie [connectedanimationservice. getanimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) mit dem eindeutigen Schlüssel aufrufen, den Sie beim Erstellen der Animation angegeben haben.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Vorwärtsnavigation

In diesem Beispiel wird gezeigt, wie connectedanimationservice verwendet wird, um einen Übergang für die Vorwärtsnavigation zwischen zwei Seiten (Page_A Page_B) zu erstellen.

Die empfohlene Animations Konfiguration für die Vorwärtsnavigation ist " [gravityconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)". Dies ist die Standardeinstellung, sodass Sie die [Konfigurations](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) Eigenschaft nur dann festlegen müssen, wenn Sie eine andere Konfiguration angeben möchten.

Richten Sie die Animation auf der Seite Quelle ein.

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

Starten Sie die Animation auf der Zielseite.

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>Rückwärts Navigation

Für die Rück Navigation (Page_B zu Page_A) führen Sie die gleichen Schritte aus, aber die Quell-und Zielseiten werden umgekehrt.

Wenn der Benutzer zurück navigiert, erwarten Sie, dass die APP so bald wie möglich in den vorherigen Zustand zurückversetzt wird. Daher ist die empfohlene Konfiguration [directconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Diese Animation ist schneller, direkter und verwendet die Verlangsamung der Verlangsamung.

Richten Sie die Animation auf der Seite Quelle ein.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Starten Sie die Animation auf der Zielseite.

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

Zwischen dem Zeitpunkt der Einrichtung und dem Start der Animation erscheint das Quell Element über der anderen Benutzeroberfläche in der APP fixiert. Auf diese Weise können Sie alle anderen Übergangs Animationen gleichzeitig ausführen. Aus diesem Grund sollten Sie nicht mehr als ~ 250 Millisekunden zwischen den beiden Schritten warten, da das vorhanden sein des Quell Elements möglicherweise ablenkend ist. Wenn Sie eine Animation vorbereiten und Sie nicht innerhalb von drei Sekunden starten, gibt das System die Animation aus, und alle nachfolgenden Aufrufe von [trystart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) können nicht ausgeführt werden.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Verbundene Animation in der Listen-und Raster Umgebung

Häufig empfiehlt es sich, eine verbundene Animation von oder zu einer Liste oder einem Raster Steuerelement zu erstellen. Um diesen Prozess zu vereinfachen, können Sie die beiden Methoden für " [ListView](/uwp/api/windows.ui.xaml.controls.listview) " und " [GridView](/uwp/api/windows.ui.xaml.controls.gridview)", " [prepareconnectedanimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) " und " [trystartconnectedanimationasync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)" verwenden.

Angenommen, Sie verfügen über ein **ListView** -Element, das ein Element mit dem Namen "portraitellipse" in seiner Daten Vorlage enthält.

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Um eine verbundene Animation mit der Ellipse vorzubereiten, die einem bestimmten Listenelement entspricht, nennen Sie die [prepareconnectedanimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) -Methode mit einem eindeutigen Schlüssel, dem Element und dem Namen "portraitellipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Verwenden Sie [trystartconnectedanimationasync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), um eine Animation mit diesem Element als Ziel zu starten, z. b. Wenn Sie aus einer Detailansicht zurück navigieren. Wenn Sie gerade die Datenquelle für ListView geladen haben, wartet trystartconnectedanimationasync, dass die Animation gestartet wird, bis der entsprechende Element Container erstellt wurde.

```csharp
private async void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation =
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>Koordinierte Animation

![Koordinierte Animation](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

Eine *koordinierte Animation* ist eine besondere Art von Eingangs Animation, bei der ein Element zusammen mit dem verbundenen Animations Ziel angezeigt wird, das zusammen mit dem verbundenen Animations Element animiert wird, während es auf dem Bildschirm bewegt wird. Koordinierte Animationen können einem Übergang mehr visuellen Interesse verleihen und die Aufmerksamkeit des Benutzers auf den Kontext, der von den Quell-und Ziel Ansichten gemeinsam genutzt wird, weiter zeichnen. In diesen Bildern wird die Benutzeroberfläche der Beschriftung für das Element mithilfe einer koordinierten Animation animiert.

Wenn eine koordinierte Animation die Gravitations Konfiguration verwendet, wird die Schwerkraft sowohl auf das verbundene Animations Element als auch auf die koordinierten Elemente angewendet. Die koordinierten Elemente werden neben dem verbundenen Element "Swoop", sodass die Elemente wirklich koordiniert bleiben.

Verwenden Sie die Überladung von **trystart** mit zwei Parametern, um einer verbundenen Animation koordinierte Elemente hinzuzufügen. Dieses Beispiel veranschaulicht eine koordinierte Animation eines Raster Layouts namens "descriptionroot", die zusammen mit einem verbundenen Animations Element mit dem Namen "coverimage" eingegeben wird.

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");

    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>Do es und 't ' TS

- Verwenden Sie eine verbundene Animation in Seiten Übergängen, bei der ein Element von den Quell-und Zielseiten gemeinsam genutzt wird.
- Verwenden Sie " [gravityconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) " für die Vorwärtsnavigation.
- Verwenden Sie [directconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) für die Rück Navigation.
- Warten Sie nicht auf Netzwerk Anforderungen oder andere asynchrone Vorgänge mit langer Ausführungszeit zwischen dem vorbereiten und Starten einer verbundenen Animation. Möglicherweise müssen Sie die erforderlichen Informationen vorab laden, um den Übergang vorab auszuführen, oder Sie verwenden ein Platzhalter Bild mit niedriger Auflösung, während ein hochauflösende Bild in der Ziel Ansicht geladen wird.
- Verwenden Sie [suppressnavigationtransitioninfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) , um eine Übergangs Animation in einem **Frame** zu verhindern, wenn Sie **connectedanimationservice**verwenden, da verbundene Animationen nicht zur gleichzeitigen Verwendung mit den Standard Navigations Übergängen gedacht sind. Weitere Informationen zur Verwendung von Navigations Übergängen finden Sie unter [navigationdermetransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) .

## <a name="related-articles"></a>Verwandte Artikel

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[Connectedanimationservice](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)