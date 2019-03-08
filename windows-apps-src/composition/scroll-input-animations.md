---
title: Erweitern vorhandener ScrollViewer-Erfahrungen
description: Lernen Sie, wie Sie mit einem XAML-ScrollViewer und ExpressionAnimations dynamische, eingabegesteuerte Bewegungserlebnisse erzeugen können.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 118b3f6e306e60d1d8d569f0d58f2d77ea30d9a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635065"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Erweitern vorhandener ScrollViewer-Erfahrungen

Dieser Artikel erklärt, wie Sie mit einem XAML-ScrollViewer und ExpressionAnimations dynamische, eingabegesteuerte Bewegungserlebnisse erzeugen können.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Eingabe-driven Animationen](input-driven-animations.md)
- [Beziehung basierend Animationen](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>Warum auf ScrollViewer aufbauen?

Normalerweise verwenden Sie den vorhandenen XAML ScrollViewer, um eine scrollbare und zoombare Oberfläche für Ihre App-Inhalte zu erstellen. Mit der Einführung der Fluent Design-Sprache sollten Sie sich nun auch darauf konzentrieren, wie Sie den Akt des Scrollens oder Zooms einer Oberfläche nutzen können, um andere Bewegungserlebnisse zu steuern. Zum Beispiel, mit Scrolling eine unscharfe Animation eines Hintergrunds zu nutzen oder die Position eines „Sticky-Headers“ zu steuern.

In diesen Szenarien nutzen Sie das Verhalten oder Manipulations-Erlebnisse wie Scrolling und Zoomen, um andere Teile Ihrer Anwendung dynamischer zu gestalten. Diese ermöglichen es der App, sich in sich geschlossener anzufühlen und die Erlebnisse in den Augen der Endbenutzer noch einprägsamer zu gestalten. Durch die Verbesserung der Benutzeroberfläche der App werden Endbenutzer häufiger und länger mit der App in Kontakt treten.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>Was können Sie auf ScrollViewer aufbauen?

Sie können die Position eines ScrollViewer nutzen, um eine Reihe dynamischer Erlebnisse zu erstellen:

- Parallax – Nutzen Sie die Position eines ScrollViewer, um den Hintergrund- oder Vordergrundinhalte relativ schnell auf die Scrollposition zu verschieben.
- StickyHeaders – Verwenden Sie die Position eines ScrollViewer, um einen Header zu animieren und an eine Position zu „kleben“.
- Input-Driven Effects – Verwenden Sie die Position eines Scrollviewer, um einen Kompositionseffekt wie z. B. Blur zu animieren.

Im Allgemeinen ist es möglich, durch Referenzieren der Position eines ScrollViewer mit einer ExpressionAnimation eine Animation zu erstellen, die sich dynamisch relativ zum Scroll-Betrag ändert.

![Listenansicht mit Parallax](images/animation/parallax.gif)

![Ein Shy-Header](images/animation/shy-header.gif)

## <a name="using-scrollmanipulationpropertyset"></a>Verwenden von ScrollManipulationPropertySet

Um diese dynamischen Erlebnisse mit einem XAML ScrollViewer zu erzeugen, müssen Sie die Scroll-Position in einer Animation referenzieren können. Dies geschieht durch den Zugriff auf ein CompositionPropertySet aus dem XAML ScrollViewer, das ScrollManipulationPropertySet.
Das ScrollManipulationPropertySet enthält eine einzige Vector3-Eigenschaft namens Translation, die Zugriff auf die Scroll-Position des ScrollViewer ermöglicht. Sie können diese dann wie jedes andere CompositionPropertySet in Ihrer ExpressionAnimation referenzieren.

Allgemeine Schritte zum Einstieg:

1. Greifen Sie über ElementCompositionPreview auf ScrollManipulationPropertySet zu.
    - `ElementCompositionPreview.GetScrollManipulationPropertySet(ScrollViewer scroller)`
1. Erstellen Sie eine ExpressionAnimation, die auf die Eigenschaft Translation des PropertySet verweist.
    - Vergessen Sie nicht, den Referenzparameter festzulegen!
1. Mit der ExpressionAnimation können Sie die Eigenschaft eines CompositionObjects gezielt ansteuern.

> [!NOTE]
> Es wird empfohlen, das PropertySet, das von der Methode GetScrollManipulationPropertySet zurückgegeben wird, einer Klassenvariablen zuzuordnen. Dies stellt sicher, dass das Property-Set nicht durch die Garbage-Collection bereinigt wird und hat somit keinen Einfluss auf die ExpressionAnimation, in der es referenziert wird. ExpressionAnimations hat keine starke Referenz auf die in der Gleichung verwendeten Objekte.

## <a name="example"></a>Beispiel

Schauen wir uns an, wie das oben gezeigte Parallax-Beispiel zusammengesetzt ist. Als Referenz finden Sie den gesamten Quellcode der App im [Window UI Dev Labs-Repo auf GitHub](https://github.com/Microsoft/WindowsUIDevLabs).

Das Erste ist, eine Referenz auf das ScrollManipulationPropertySet zu erhalten.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

Der nächste Schritt ist die ExpressionAnimation, die eine Gleichung definiert, die die Scroll-Position des ScrollViewer verwendet.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> Sie können auch die ExpressionBuilder-Helper-Klassen verwenden, um denselben Ausdruck ohne Zeichenfolgen zu konstruieren:

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Schließlich nehmen Sie diese ExpressionAnimation und zielen auf das Visual, dass Sie per Parallax animieren wollen. In diesem Fall ist es das Bild für jeden Eintrag in der Liste.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```