---
title: Verbessern vorhandener ScrollViewer-Erfahrungen
description: Erfahren Sie, wie Sie mit XAML ScrollViewer und expressionanimationen dynamische Eingabe gesteuerte Bewegungsmöglichkeiten erstellen.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: 438f108a07349da6515443e64bd4494529b8e6a0
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152403"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Verbessern vorhandener ScrollViewer-Erfahrungen

In diesem Artikel wird erläutert, wie Sie XAML ScrollViewer und expressionanimationen verwenden, um dynamische Eingabe gesteuerte Bewegungs Umgebungen zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie mit den Konzepten vertraut sind, die in den folgenden Artikeln erläutert werden:

- [Eingabegesteuerte Animationen](input-driven-animations.md)
- [Beziehungs basierte Animationen](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>Warum auf ScrollViewer aufbauen?

In der Regel verwenden Sie den vorhandenen XAML ScrollViewer, um eine Bild Lauf Bare und Zoombare Oberfläche für Ihren app-Inhalt zu erstellen. Mit der Einführung der fließenden Entwurfs Sprache sollten Sie sich nun auch darauf konzentrieren, wie Sie den Bildlauf oder das Zoomen einer Oberfläche verwenden, um andere Bewegungsmöglichkeiten zu fördern. Beispielsweise können Sie mit einem Bildlauf eine unscharfe Animation eines Hintergrunds verwenden oder die Position eines "Kurznotiz Headers" steuern.

In diesen Szenarien nutzen Sie das Verhalten oder die Manipulation von Benutzeroberflächen, wie z. b. durch Scrollen und Zoomen, um andere Teile der APP dynamischer zu gestalten. Diese wiederum ermöglichen es der APP, mehr zusammenhängend zu gestalten, sodass die Erlebnisse in den Augen von Endbenutzern noch wichtiger werden. Wenn Sie die Benutzeroberfläche der APP einprägsamer gestalten, werden Endbenutzer häufiger und für längere Zeiträume mit der app in Kontakt treten.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>Was können Sie auf ScrollViewer aufbauen?

Sie können die Position eines ScrollViewer nutzen, um eine Reihe dynamischer Umgebungen zu erstellen:

- Parser – verwenden Sie die Position eines ScrollViewer-Steuerelement, um Hintergrund-oder Vordergrund Inhalt in relativer Geschwindigkeit an die Scrollposition zu verschieben.
- "Stickyheaders" – Verwenden der Position eines ScrollViewer zum animieren und "anheften" eines Headers an eine Position
- Input-Driven Effekte – verwenden Sie die Position eines ScrollViewer, um einen Kompositions Effekt wie z. b. weich zu animieren.

Im Allgemeinen können Sie durch das verweisen auf die Position eines ScrollViewer-Ausdrucks mit einer expressionanimation eine Animation erstellen, die die relative Bild Lauf Menge dynamisch ändert.

![Listenansicht mit "initiallax"](images/animation/parallax.gif)

![Ein Scheuer Header](images/animation/shy-header.gif)

## <a name="using-scrollviewermanipulationpropertyset"></a>Verwenden von scrollviewermanipulationpropertyset

Um diese dynamischen Umgebungen mit einem XAML ScrollViewer zu erstellen, müssen Sie in der Lage sein, auf die Scrollposition in einer Animation zu verweisen. Dies erfolgt durch den Zugriff auf ein compositionpropertyset aus dem XAML ScrollViewer, der als scrollviewermanipulationpropertyset bezeichnet wird.
Scrollviewermanipulationpropertyset enthält eine einzelne Vector3-Eigenschaft mit dem Namen Translation, die den Zugriff auf die Scrollposition von ScrollViewer ermöglicht. Sie können dann wie jedes andere compositionpropertyset in der expressionanimation referenzieren.

Allgemeine Schritte für den Einstieg:

1. Greifen Sie über elementcompositionpreview auf das scrollviewermanipulationpropertyset zu.
    - `ElementCompositionPreview.GetScrollViewerManipulationPropertySet(ScrollViewer scroller)`
1. Erstellen Sie eine expressionanimation, die auf die Translation-Eigenschaft aus dem PropertySet verweist.
    - Vergessen Sie nicht, den Verweis Parameter festzulegen.
1. Richten Sie die-Eigenschaft eines compositionobject mit der expressionanimation ein.

> [!NOTE]
> Es wird empfohlen, das von der getscrollviewermanipulationpropertyset-Methode zurückgegebene PropertySet einer Klassen Variablen zuzuweisen. Dadurch wird sichergestellt, dass der Eigenschaften Satz nicht von der Garbage Collection bereinigt wird und somit keine Auswirkung auf die expressionanimation hat, in der auf Sie verwiesen wird. Expressionanimationen behalten keinen starken Verweis auf eines der in der Gleichung verwendeten-Objekte bei.

## <a name="example"></a>Beispiel

Sehen wir uns nun an, wie das oben gezeigte Beispiel oben dargestellt wird. Als Referenz finden Sie den gesamten Quellcode für die APP im Repository " [Windows UI dev Labs" auf GitHub](https://github.com/microsoft/WindowsCompositionSamples).

Der erste Schritt besteht darin, einen Verweis auf das scrollviewermanipulationpropertyset zu erhalten.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

Der nächste Schritt ist das Erstellen der expressionanimation, die eine Gleichung definiert, die die Bild Lauf Position von ScrollViewer verwendet.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> Sie können auch ExpressionBuilder-Hilfsklassen verwenden, um denselben Ausdruck zu erstellen, ohne dass Zeichen folgen erforderlich sind:

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Schließlich nehmen Sie diese expressionanimation an und Zielen auf das visuelle Element ab, das Sie verwenden möchten. In diesem Fall ist es das Bild für jedes der Elemente in der Liste.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```
