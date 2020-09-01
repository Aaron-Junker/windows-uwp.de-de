---
title: Seitenübergänge
description: Erfahren Sie, wie Sie universelle Windows-Plattform (UWP)-Seiten Übergänge verwenden, um Benutzern Feedback zur Beziehung zwischen Seiten in Ihrer APP zu geben.
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b1fc38f5224ae9627f4c793a800ab747cfa1c2b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173854"
---
# <a name="page-transitions"></a>Seitenübergänge

Seiten Übergänge Navigieren durch Benutzer zwischen Seiten in einer APP und geben Feedback als Beziehung zwischen den Seiten an. Mithilfe von Seiten Übergängen können Benutzer verstehen, ob Sie sich am Anfang einer Navigations Hierarchie befinden, sich zwischen gleich geordneten Seiten bewegen oder tiefer in die Seiten Hierarchie navigieren.

Zwei verschiedene Animationen werden für die Navigation zwischen Seiten in einer APP, *Seiten Aktualisierung* und Drilldown bereit *gestellt und durch*Unterklassen von [**navigationtransitioninfo**](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo)dargestellt.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die Katalog-App für <strong style="font-weight: semi-bold">XAML</strong> -Steuerelemente installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/PageTransition">die APP zu öffnen und die Seiten Übergänge in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>Seitenaktualisierung

Bei der Seiten Aktualisierung handelt es sich um eine Kombination aus einer Bild-auf-Animation und eine einblenden Animation für den eingehenden Inhalt Verwenden Sie die Seiten Aktualisierung, wenn der Benutzer an den Anfang eines Navigations Stapels geht, z. b. durch Navigieren zwischen Registerkarten oder linken Navigationselementen.

Das gewünschte Gefühl ist, dass der Benutzer schon einmal begonnen hat.

![Animation zur Seiten Aktualisierung](images/page-refresh.gif)

Die Animation zur Seiten Aktualisierung wird durch die [**Klasse "entrancenavigationtransitioninfoclass**](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo)" dargestellt.

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Hinweis**: ein [**Frame**](/uwp/api/windows.ui.xaml.controls.frame) verwendet automatisch [**navigationdermetransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) , um die Navigation zwischen zwei Seiten zu animieren. Die Animation ist standardmäßig eine Seiten Aktualisierung.

## <a name="drill"></a>Drill

Verwenden Sie Drill, wenn Benutzer in einer APP tiefer navigieren, z. b. nach dem Auswählen eines Elements Weitere Informationen anzeigen.

Das gewünschte Gefühl besteht darin, dass der Benutzer die APP weiter vertieft hat.

![Drill Animation](images/drill.gif)

Die Drill Animation wird durch die [**drillinnavigationtransitioninfo**](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) -Klasse dargestellt.

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Horizontale Folie

Verwenden Sie die horizontale Folie, um anzuzeigen, dass neben geordnete Seiten nebeneinander angezeigt werden. Das [navigationview](../controls-and-patterns/navigationview.md) -Steuerelement verwendet diese Animation automatisch für den oberen Navigationsbereich. Wenn Sie jedoch eine eigene horizontale Navigation entwickeln, können Sie eine horizontale Folie mit slidenavigationtransitioninfo implementieren.

Das gewünschte Gefühl ist, dass der Benutzer zwischen Seiten navigiert, die sich nebeneinander befinden. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Suppress

Um die Wiedergabe von Animationen während der Navigation zu vermeiden, verwenden Sie [**suppressnavigationtransitioninfo**](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) anstelle anderer **navigationtransitioninfo** -Untertypen.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

Das Unterdrücken der Animation ist hilfreich, wenn Sie mit [verbundenen Animationen](connected-animation.md) oder impliziten Show/Hide-Animationen einen eigenen Übergang erstellen.

## <a name="backwards-navigation"></a>Rückwärtsnavigation

Sie können verwenden `Frame.GoBack(NavigationTransitionInfo)` , um einen bestimmten Übergang wiederzugeben, wenn Sie rückwärts navigieren.

Dies kann hilfreich sein, wenn Sie das Navigationsverhalten dynamisch basierend auf der Bildschirmgröße ändern. beispielsweise in einem reaktionsfähigen Master-/Detail-Szenario.

## <a name="related-topics"></a>Zugehörige Themen

- [Navigieren zwischen zwei Seiten](../basics/navigate-between-two-pages.md)
- [Bewegung in uwindowswp-apps](index.md)