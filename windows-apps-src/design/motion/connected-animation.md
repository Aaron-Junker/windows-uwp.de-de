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
ms.openlocfilehash: 6e17b1c18fc8e643ac788e5e13ac78cae49a35ef
ms.sourcegitcommit: 6d743cf9c3e09f87ea2879b8e1f2dc4a1b1a16fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74166079"
---
# <a name="connected-animation-for-uwp-apps"></a>Verbundene Animation für UWP-Apps

Mit verbundenen Animationen können Sie dynamische und ansprechende Navigationsfunktionen erstellen, indem Sie den Übergang eines Elements zwischen zwei verschiedenen Ansichten animieren. So können Benutzer den Kontext beibehalten, und es entsteht Kontinuität zwischen den Ansichten.

In einer verbundenen Animation scheint ein Element während einer Änderung des UI-Inhalts "Fortfahren" zwischen zwei Ansichten zu wechseln, und über den Bildschirm von seiner Position in der Quell Ansicht bis zum Ziel in der neuen Ansicht zu wechseln. Dadurch wird der gemeinsame Inhalt zwischen den Ansichten hervorgehoben, und es wird ein schöner und dynamischer Effekt als Teil eines Übergangs erzeugt.

> **Wichtige APIs**: [connectedanimation Class](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [connectedanimationservice-Klasse](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


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

In diesem kurzen Video wird eine APP mit einer verbundenen Animation zum Animieren eines Element Bilds verwendet, da es fortgesetzt wird, um Teil des Headers der nächsten Seite zu werden. Dieser Effekt trägt dazu bei, Benutzerkontext beim Übergang beizubehalten.

![Verbundene Animationen](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Videozusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Verbundene Animationen und das Fluent Design-System

 Mit dem Fluent Design-System können Sie moderne, klare Benutzeroberflächen erstellen, die Licht, Tiefe, Bewegung, Material und Skalierung enthalten. Verbundene Animation ist eine Komponente des Fluent Design-Systems, die Bewegung in Ihre App bringt. Weitere Informationen finden Sie in der [Übersicht über Fluent Design für UWP](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>Warum eine verbundene Animation?

Beim Navigieren zwischen Seiten ist es wichtig, dass der Benutzer versteht, welcher neue Inhalt nach der Navigation dargestellt wird und in welcher Beziehung dieser zu seiner Absicht beim Navigieren steht. Verbundene Animationen bieten eine leistungsstarke visuelle Metapher, die die Beziehung zwischen zwei Ansichten hervorhebt, indem sie den Fokus des Benutzers auf den gemeinsamen Inhalt zwischen diesen beiden lenkt. Darüber hinaus fügen verbundene Animationen der Seitennavigation visuelle Spannung und einen Feinschliff hinzu, die dazu beitragen können, dass sich die Bewegungsgestaltung Ihrer App von anderen unterscheidet.

## <a name="when-to-use-connected-animation"></a>Verwendungsszenarien für verbundene Animationen

Verbundene Animationen werden in der Regel bei einem Seitenwechsel verwendet, sie können jedoch immer verwendet werden, wenn Sie Inhalt in einer UI ändern und möchten, dass der Benutzerkontext beibehalten wird. Sie sollten es in Betracht ziehen, eine verbundene Animation anstelle eines [Drills in einem Navigationsübergangs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) zu verwenden, wenn die Quell- und die Zielansicht ein gemeinsames Bild oder ein anderes gemeinsames UI-Element aufweisen.

## <a name="configure-connected-animation"></a>Verbundene Animation konfigurieren

> [!IMPORTANT]
> Für dieses Feature ist es erforderlich, dass die Zielversion Ihrer APP Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher ist. Die-Konfigurations Eigenschaft ist in früheren sdert nicht verfügbar. Sie können eine minimale Version als SDK 17763 als Ziel verwenden, indem Sie adaptiven Code oder bedingtes XAML verwenden. Weitere Informationen finden Sie unter [Version Adaptive apps](/windows/uwp/debug-test-perf/version-adaptive-apps).

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
| Schwerkraft | „Ja“ | Ja* <br/> **die grundlegende Übersetzung von A zu B verwendet diese Beschleunigungs Funktion, aber die "Gravity Dip" hat eine eigene Beschleunigungs Funktion.*  |
| Unmittelbaren | Nein <br/> *Animiert mehr als 150 ms.*| Nein <br/> *Verwendet die verlangsamnde Beschleunigungs Funktion.* |
| Basic | „Ja“ | „Ja“ |

## <a name="how-to-implement-connected-animation"></a>Implementieren einer verbundenen Animation

Das Einrichten einer verbundenen Animation besteht aus zwei Schritten:

1. *Bereiten* Sie ein Animations Objekt auf der Seite Quelle vor, das dem System anzeigt, dass das Quell Element an der verbundenen Animation beteiligt ist.
1. *Starten* Sie die Animation auf der Zielseite, und übergeben Sie einen Verweis auf das Ziel Element.

Wenn Sie von der Seite "Quelle" aus navigieren, können Sie [connectedanimationservice. getforcurrentview](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) aufrufen, um eine Instanz von connectedanimationservice abzurufen. Um eine Animation vorzubereiten, nennen Sie [preparetoanimieren](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) für diese Instanz, und übergeben Sie einen eindeutigen Schlüssel und das UI-Element, das Sie im Übergang verwenden möchten. Mit dem eindeutigen Schlüssel können Sie die Animation später auf der Zielseite abrufen.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Wenn die Navigation erfolgt, starten Sie die Animation auf der Zielseite. Rufen Sie zum Starten der Animation [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) auf. Sie können die richtige Animationsinstanz abrufen, indem Sie mit dem eindeutigen Schlüssel, den Sie beim Erstellen der Animation angegeben haben, [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) aufrufen.

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

Zwischen dem Zeitpunkt der Einrichtung und dem Start der Animation erscheint das Quell Element über der anderen Benutzeroberfläche in der APP fixiert. Auf diese Weise können Sie alle anderen Übergangs Animationen gleichzeitig ausführen. Aus diesem Grund sollten Sie nicht mehr als ~ 250 Millisekunden zwischen den beiden Schritten warten, da das vorhanden sein des Quell Elements möglicherweise ablenkend ist. Wenn Sie eine Animation vorbereiten und diese nicht innerhalb von drei Sekunden starten, wird sie vom System gelöscht und alle folgenden Aufrufe von [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) schlagen fehl.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Verbundene Animationen in Listen- und Rasterumgebungen

Sie werden wahrscheinlich häufig verbundene Animationen aus einem Listen- oder Rastersteuerelement erstellen wollen. Um diesen Prozess zu vereinfachen, können Sie die beiden Methoden für " [ListView](/uwp/api/windows.ui.xaml.controls.listview) " und " [GridView](/uwp/api/windows.ui.xaml.controls.gridview)", " [prepareconnectedanimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) " und " [trystartconnectedanimationasync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)" verwenden.

Nehmen wir beispielsweise an, Sie haben eine **ListView**, die ein Element mit dem Namen „PortraitEllipse” in der Datenvorlage enthält.

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
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
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

Eine *koordinierte Animation* ist eine besondere Art von Eingangs Animation, bei der ein Element zusammen mit dem verbundenen Animations Ziel angezeigt wird, das zusammen mit dem verbundenen Animations Element animiert wird, während es auf dem Bildschirm bewegt wird. Koordinierte Animationen können einem Übergang weitere visuelle Spannung hinzufügen und die Aufmerksamkeit des Benutzers auf den Kontext lenken, den die Quell- und die Zielansicht teilen. In diesen Bildern wird die Beschriftungs-UI für das Element mithilfe einer koordinierten Animation animiert.

Wenn eine koordinierte Animation die Gravitations Konfiguration verwendet, wird die Schwerkraft sowohl auf das verbundene Animations Element als auch auf die koordinierten Elemente angewendet. Die koordinierten Elemente werden neben dem verbundenen Element "Swoop", sodass die Elemente wirklich koordiniert bleiben.

Verwenden Sie die Überladung von **TryStart** mit zwei Parametern, um koordinierte Elemente zu einer verbundenen Animation hinzuzufügen. Dieses Beispiel veranschaulicht eine koordinierte Animation eines Raster Layouts namens "descriptionroot", die zusammen mit einem verbundenen Animations Element mit dem Namen "coverimage" eingegeben wird.

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

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden Sie eine verbundene Animation bei Seitenübergängen, bei denen ein Element zwischen der Quell- und der Zielseite geteilt wird.
- Verwenden Sie " [gravityconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) " für die Vorwärtsnavigation.
- Verwenden Sie [directconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) für die Rück Navigation.
- Warten Sie nicht auf Netzwerk Anforderungen oder andere asynchrone Vorgänge mit langer Ausführungszeit zwischen dem vorbereiten und Starten einer verbundenen Animation. Möglicherweise müssen Sie die erforderlichen Informationen laden, um den Übergang vorab auszuführen, oder ein Platzhalterbild mit niedriger Auflösung verwenden, während ein Bild mit hoher Auflösung in der Zielansicht geladen wird.
- Verwenden Sie [suppressnavigationtransitioninfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) , um eine Übergangs Animation in einem **Frame** zu verhindern, wenn Sie **connectedanimationservice**verwenden, da verbundene Animationen nicht zur gleichzeitigen Verwendung mit den Standard Navigations Übergängen gedacht sind. Weitere Informationen zur Verwendung von Navigationsübergängen finden Sie unter [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition).

## <a name="related-articles"></a>Verwandte Artikel

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[Connectedanimationservice](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[Navigationermetransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
