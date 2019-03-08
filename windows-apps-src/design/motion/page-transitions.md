---
Description: Erfahren Sie, wie Seitenübergänge in Ihren UWP-apps verwenden.
title: Seitenübergänge in UWP-Apps
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 38fe6b92828459f91ba6ea2f836d274c2cc8d761
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646455"
---
# <a name="page-transitions"></a>Seitenübergänge

Seitenübergänge sind Animationen, die abgespielt werden, wenn Benutzer zwischen Seiten in einer App navigieren und Feedback als Beziehung zwischen Seiten liefern. Seitenübergänge zeigen dem Benutzer, ob er an der Spitze einer Navigationshierarchie steht, zwischen Geschwisterseiten wechselt oder tiefer in die Seitenhierarchie navigiert.

Für die Navigation zwischen Seiten in einer App stehen zwei verschiedene Animationen zur Verfügung: *Seitenaktualisierung* und *Drill*. Sie werden durch Unterklassen von [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) dargestellt.

## <a name="page-refresh"></a>Seite aktualisieren

Die Seitenaktualisierung ist eine Kombination aus einer Folie Animation und eine Überblendung in die Animation für den eingehenden Inhalt. Verwenden Sie die Seitenaktualisierung, wenn der Benutzer an den Anfang eines Navigationsstapels gebracht wird, z. B. beim Navigieren zwischen Registerkarten oder Navigationselementen auf der linken Navigationsleiste.

Der gewünschte Effekt ist ein Neubeginn für den Benutzer.

![Seitenaktualisierungsanimation](images/page-refresh.gif)

Die Seitenaktualisierungsanimation wird durch die Klasse [**EntranceNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) dargestellt.

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Hinweis**: Ein [ **Frame** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) verwendet automatisch die [ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) Navigation zwischen zwei Seiten animiert. Standardmäßig ist die Animation eine Seitenaktualisierung.

## <a name="drill"></a>Drill

Verwenden Sie Drill, wenn Benutzer tiefer in eine Anwendung navigieren, z. B. um nach der Auswahl eines Elements weitere Informationen anzuzeigen.

Der gewünschte Effekt ist, dass der Benutzer tiefer in die App vorgedrungen ist.

![Drill-Animation](images/drill.gif)

Die Drill-Animation wird durch die Klasse [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) dargestellt.

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Horizontale Folie

Verwenden Sie horizontale Folie, um anzuzeigen, dass gleichgeordneten Seiten nebeneinander angezeigt werden. Die [NavigationView](../controls-and-patterns/navigationview.md) Steuerelement wird automatisch diese Animation für Top Nav verwendet, aber wenn Sie Ihre eigenen horizontale Navigationserlebnis erstellen, dann können Sie implementieren horizonal Folie mit SlideNavigationTransitionInfo.

Die gewünschte Gefühl ist, dass der Benutzer Navigieren zwischen Seiten, die nebeneinander befinden. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Unterdrückung

Um die Wiedergabe von Animationen während der Navigation zu vermeiden, verwenden Sie [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) anstelle anderer **NavigationTransitionInfo**-Subtypen.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

Das Unterdrücken der Animation ist hilfreich, wenn Sie Ihre eigenen Übergang mit [verbundenen Animationen](connected-animation.md) erstellen oder das implizite Einblenden/Ausblenden von Animationen nutzen.

## <a name="backwards-navigation"></a>Rückwärtsnavigation

Um einen bestimmten Übergang bei der Rückwärtsnavigation darzustellen, verwenden Sie `Frame.GoBack(NavigationTransitionInfo)`.

Dies kann nützlich sein, wenn Sie das Navigationsverhalten dynamisch an die Bildschirmgröße anpassen – z. B. in einem dynamischen Master/Detail-Szenario.

## <a name="related-topics"></a>Verwandte Themen

- [Navigieren Sie zwischen zwei Seiten](../basics/navigate-between-two-pages.md)
- [Während der Übertragung in UWP-apps](index.md)
