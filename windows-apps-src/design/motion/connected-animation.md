---
description: Mit verbundenen Animationen können Sie eine dynamische und ansprechende Navigationsfunktionalität erstellen, indem Sie den Übergang eines Elements zwischen zwei verschiedenen Ansichten animieren.
title: Verbundene Animationen
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 21e7c026d336507b1a82badba770ac3bb50e19f8
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984120"
---
# <a name="connected-animation-for-uwp-apps"></a>Verbundene Animation für UWP-Apps

Mit verbundenen Animationen können Sie eine dynamische und ansprechende Navigationsfunktionalität erstellen, indem Sie den Übergang eines Elements zwischen zwei verschiedenen Ansichten animieren. So können Benutzer den Kontext beibehalten, und es entsteht Kontinuität zwischen den Ansichten.

In einer verbundenen-Animation scheint ein Element zwischen zwei Ansichten, während eine Änderung der Inhalte der Benutzeroberfläche, über den Bildschirm aus seiner Position in der Ansicht an das Ziel in der neuen Sicht fliege "Weiter". Dies betont die gemeinsamen Inhalte zwischen den Ansichten und eine ansprechende und dynamische Auswirkungen im Rahmen eines Übergangs.

> **Wichtige APIs:**  [ConnectedAnimation Klasse](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [ConnectedAnimationService-Klasse](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ConnectedAnimation">öffnen Sie die app und die verbundene Animation in Aktion erleben</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

In diesem kurzen Video verwendet eine app eine verbundene Animation, um ein Elementbild zu animieren, wie "so lange" Teil des Headers der nächsten Seite. Dieser Effekt trägt dazu bei, Benutzerkontext beim Übergang beizubehalten.

![Verbundene Animationen](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Video-Zusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Verbundene Animationen und das Fluent Design-System

 Mit dem Fluent Design-System erstellen Sie moderne Oberflächen, die Licht, Tiefe, Bewegung, Material und Skalierungsmöglichkeiten beinhalten. Verbundene Animation ist eine Komponente des Fluent Design-Systems, die Bewegung in Ihre App bringt. Weitere Informationen finden Sie in der [Fluent Design für UWP-Übersicht](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>Warum eine verbundene Animation?

Beim Navigieren zwischen Seiten ist es wichtig, dass der Benutzer versteht, welcher neue Inhalt nach der Navigation dargestellt wird und in welcher Beziehung dieser zu seiner Absicht beim Navigieren steht. Verbundene Animationen bieten eine leistungsstarke visuelle Metapher, die die Beziehung zwischen zwei Ansichten hervorhebt, indem sie den Fokus des Benutzers auf den gemeinsamen Inhalt zwischen diesen beiden lenkt. Darüber hinaus fügen verbundene Animationen der Seitennavigation visuelle Spannung und einen Feinschliff hinzu, die dazu beitragen können, dass sich die Bewegungsgestaltung Ihrer App von anderen unterscheidet.

## <a name="when-to-use-connected-animation"></a>Verwendungsszenarien für verbundene Animationen

Verbundene Animationen werden in der Regel bei einem Seitenwechsel verwendet, sie können jedoch immer verwendet werden, wenn Sie Inhalt in einer UI ändern und möchten, dass der Benutzerkontext beibehalten wird. Sie sollten es in Betracht ziehen, eine verbundene Animation anstelle eines [Drills in einem Navigationsübergangs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) zu verwenden, wenn die Quell- und die Zielansicht ein gemeinsames Bild oder ein anderes gemeinsames UI-Element aufweisen.

## <a name="configure-connected-animation"></a>Konfigurieren Sie die verbundene animation

> [!IMPORTANT]
> Dieses Feature erfordert, dass Zielversion Ihrer app Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher. Die Configuration-Eigenschaft ist nicht in frühere SDKs zur Verfügung. Sie können eine Mindestversion von niedriger als SDK 17763 abzielen mit adaptiven Code oder bedingte XAML. Weitere Informationen finden Sie unter [versionsabhängig adaptive apps](/windows/uwp/debug-test-perf/version-adaptive-apps).

Ab Windows 10, Version 1809, weitere verbundene Animationen enthalten Fluent-Entwurf durch die Bereitstellung der Animation Konfigurationen zugeschnitten sind speziell für die Vorwärts und rückwärts navigieren.

Geben Sie eine Animation-Konfiguration durch Festlegen der Konfigurationseigenschaft für die ConnectedAnimation. (Wir zeigen Beispiele im nächsten Abschnitt.)

Diese Tabelle beschreibt die verfügbaren Konfigurationen. Weitere Informationen zu den Motion-Prinzipien, die in diese Animationen angewendet, finden Sie unter [Direktionalität und die Schwerkraft](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Dies ist die Standardkonfiguration, und es wird empfohlen, für die Vorwärtsnavigation. |
Während der Benutzer die Weiterleitung in der app (A, B) navigiert, wird das Element verbundenen zu physisch "aus der Seite" pull"angezeigt. In diesem Fall wird das Element angezeigt wird, um vorwärts in Z-Raum zu wechseln und ein wenig als Folge der Schwerkraft halten und löscht. Um die Auswirkungen der Schwerkraft zu umgehen, wird das Element erhält der Geschwindigkeit und beschleunigt die in der letzten Position. Das Ergebnis ist eine Animation "skalieren und DIP-Adresse". |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| Während der Benutzer rückwärts in der app (B, A) navigiert, wird die Animation mehr direkt. Das Element verbundenen übersetzt linear von B an einem mit einem Decelerate kubische Bezier Beschleunigungsfunktion. Die Abwärtskompatibilität visuelle Unterstützung gibt zurück, den Benutzer in ihren vorherigen Zustand so schnell wie möglich gleichzeitig den navigationsfluss Kontext. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Dies ist die Standardeinstellung (nur und) Animation, die in Versionen vor Windows 10, Version 1809 verwendet ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService-Konfiguration

Die [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) -Klasse verfügt über zwei Eigenschaften, die für die einzelnen Animationen anstelle des gesamten Diensts gelten.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Um die unterschiedlichen Effekte zu erzielen, werden einige Konfigurationen ignorieren diese Eigenschaften für ConnectedAnimationService und stattdessen ihre eigenen Werte, wie in dieser Tabelle beschrieben.

| Konfiguration | Hinsicht DefaultDuration? | Hinsicht DefaultEasingFunction? |
| - | - | - |
| Schwerkraft | Ja | Ja* <br/> **Die einfache Übersetzung von A zu B verwendet diese Beschleunigungsfunktion, aber die "Schwerkraft DIP-Adresse" verfügt über eine eigene Beschleunigungsfunktion.*  |
| Direkte | Nein <br/> *Führt eine Animation über 150ms.*| Nein <br/> *Verwendet die Decelerate Beschleunigungsfunktion.* |
| Einfach | Ja | Ja |

## <a name="how-to-implement-connected-animation"></a>Gewusst wie: Implementieren von verbundene animation

Das Einrichten einer verbundenen Animation besteht aus zwei Schritten:

1. *Bereiten Sie vor* eines Animationsobjekts auf der Seite "Quelle", die das System anzeigt, dass das Quellelement in die verbundene Animation einbezogen werden.
1. *Starten Sie* die Animation auf der Seite "Ziel", und übergeben einen Verweis auf das Zielelement.

Wenn von der Quellseite zu navigieren, rufen Sie [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) um eine Instanz des ConnectedAnimationService abzurufen. Um eine Animation vorzubereiten, rufen [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) auf dieser Serverinstanz aus, und übergeben Sie einen eindeutigen Schlüssel und das UI-Element, das Sie beim Übergang verwenden möchten. Der eindeutige Schlüssel können Sie die Animation weiter unten auf der Seite "Ziel" abrufen.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Bei der Navigation, starten Sie die Animation in der Seite "Ziel" aus. Rufen Sie zum Starten der Animation [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) auf. Sie können die richtige Animationsinstanz abrufen, indem Sie mit dem eindeutigen Schlüssel, den Sie beim Erstellen der Animation angegeben haben, [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) aufrufen.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Vorwärtsnavigation

Dieses Beispiel zeigt, wie Sie ConnectedAnimationService zu verwenden, um einen Übergang für die Vorwärtsnavigation zwischen zwei Seiten (Page_A zu Page_B) zu erstellen.

Ist die empfohlene Animation-Konfiguration für die Vorwärtsnavigation [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration). Dies ist die Standardeinstellung, sodass Sie nicht festlegen müssen die [Konfiguration](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) Eigenschaft, wenn Sie eine andere Konfiguration angeben möchten.

Richten Sie die Animation auf der Quellseite.

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

Starten Sie die Animation in der Seite "Ziel" ein.

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

### <a name="back-navigation"></a>Rückwärtsnavigation

Für die Rückwärtsnavigation (Page_B zu Page_A), führen Sie die gleichen Schritte aus, aber die Quelle und Ziel-Seiten werden rückgängig gemacht.

Wenn der Benutzer zurück navigiert, erwarten sie, dass die app so bald wie möglich in den vorherigen Zustand zurückgegeben werden. Daher die empfohlene Konfiguration ist [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Diese Animation ist schneller, direkteren und verwendet die Decelerate vereinfachen.

Richten Sie die Animation auf der Quellseite.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Starten Sie die Animation in der Seite "Ziel" ein.

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

Zwischen dem Zeitpunkt, die die Animation eingerichtet ist und wenn sie gestartet ist wird das Quellelement eingefroren über andere Benutzeroberfläche in der app angezeigt. Dadurch können Sie alle anderen Übergangsanimationen gleichzeitig ausführen. Aus diesem Grund sollte nicht Sie mehr als ~ 250 Millisekunden zwischen den beiden Schritten warten, da das Vorhandensein des Quellelements ablenken werden kann. Wenn Sie eine Animation vorbereiten und diese nicht innerhalb von drei Sekunden starten, wird sie vom System gelöscht und alle folgenden Aufrufe von [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) schlagen fehl.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Verbundene Animationen in Listen- und Rasterumgebungen

Sie werden wahrscheinlich häufig verbundene Animationen aus einem Listen- oder Rastersteuerelement erstellen wollen. Sie können die beiden Methoden verwenden, auf [ListView](/uwp/api/windows.ui.xaml.controls.listview) und [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [: PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) und [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), Um diesen Prozess zu vereinfachen.

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

Um eine verbundene Animation mit der Ellipse, die für ein bestimmtes Listenelement vorzubereiten, rufen Sie die [: PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) -Methode mit einem eindeutigen Schlüssel, das Element und den Namen "PortraitEllipse".

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Verwenden Sie zum Starten einer Animation mit diesem Element als Ziel, z. B. wenn aus einer Detailansicht zurück navigiert [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Wenn Sie nur die Datenquelle für die ListView geladen haben, wird TryStartConnectedAnimationAsync gewartet, die Animation gestartet, bis die entsprechenden Elementcontainer erstellt wurde.

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

Ein *koordiniert Animation* ist ein spezieller Typ von "entrance" Animation, in dem ein Element angezeigt, sowie die verbundene Animation-Ziel wird, zusammen mit dem Element für die verbundene Animation animieren, wenn es auf dem Bildschirm bewegt. Koordinierte Animationen können einem Übergang weitere visuelle Spannung hinzufügen und die Aufmerksamkeit des Benutzers auf den Kontext lenken, den die Quell- und die Zielansicht teilen. In diesen Bildern wird die Beschriftungs-UI für das Element mithilfe einer koordinierten Animation animiert.

Wenn eine Animation koordinierte die Schwerkraft-Konfiguration verwendet wird, wird die Schwerkraft sowohl für das Element für die verbundene Animation als auch für die koordinierte Elemente angewendet. Die koordinierte Elemente werden zusammen mit dem Element verbundenen "swoop" werden, damit die Elemente wirklich koordinierte bleiben.

Verwenden Sie die Überladung von **TryStart** mit zwei Parametern, um koordinierte Elemente zu einer verbundenen Animation hinzuzufügen. Dieses Beispiel zeigt eine koordinierte Animation eines Grid-Layouts, die mit dem Namen "DescriptionRoot", die zusammen mit einem verbundene Animation-Element, das mit dem Namen "CoverImage" eingibt.

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
- Verwendung [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) für die Vorwärtsnavigation.
- Verwendung [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) sichern Sie bei Navigation.
- Warten Sie nicht auf netzwerkanforderungen oder andere lang ausgeführte asynchrone Vorgänge zwischen vorbereiten und eine verbundene Animation wird gestartet. Möglicherweise müssen Sie die erforderlichen Informationen laden, um den Übergang vorab auszuführen, oder ein Platzhalterbild mit niedriger Auflösung verwenden, während ein Bild mit hoher Auflösung in der Zielansicht geladen wird.
- Verwendung [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) um zu verhindern, dass eine übergangsanimation in einer **Frame** bei Verwendung von **ConnectedAnimationService**, seit verbundene Animationen sollen nicht gleichzeitig mit der Standard-Navigation-Übergänge verwendet werden. Weitere Informationen zur Verwendung von Navigationsübergängen finden Sie unter [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition).

## <a name="related-articles"></a>Verwandte Artikel

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
