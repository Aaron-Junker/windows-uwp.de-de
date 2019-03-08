---
title: Eingabegesteuerte Animationen
description: Erfahren Sie, wie Sie Eingabeanimationen in Ihrer App-Benutzeroberfläche verwenden.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 94d15fc7f2443475020aa7e134c076b833db46a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597755"
---
# <a name="input-driven-animations"></a>Eingabegesteuerte Animationen

Dieser Artikel enthält eine Einführung in die InputAnimation-API und empfiehlt, wie Sie diese Arten von Animationen in Ihrer Benutzeroberfläche verwenden können.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Beziehung basierend Animationen](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Reibungslose Bewegungsabläufe durch Benutzerinteraktion

In der Fluent Design-Sprache ist die Interaktion zwischen Endbenutzern und Apps von größter Bedeutung. Apps müssen nicht nur gut aussehen, sondern auch natürlich und dynamisch auf die Benutzer reagieren, die mit ihnen interagieren. Das bedeutet, wenn ein Finger auf dem Bildschirm platziert wird, sollte die Benutzeroberfläche auf wechselnde Eingabegrade ansprechend reagieren; das Scrollen sollte sich glatt anfühlen und an einem Finger kleben, der über den Bildschirm schwenkt.

Die Erstellung einer Benutzeroberfläche, die dynamisch und flüssig auf Benutzereingaben reagiert, führt einer besseren Einbeziehung der Benutzer. Bewegung sieht so nicht nur gut aus, sondern fühlt sich auch gut und natürlich an, wenn Benutzer mit Ihren unterschiedlichen Benutzererfahrungen interagieren. Dies ermöglicht den Endbenutzern eine einfachere Verbindung mit Ihrer Anwendung, wodurch das Erlebnis unvergesslicher und angenehmer wird.

## <a name="expanding-past-just-touch"></a>Mehr als nur Touch

Obwohl Touch eine der gebräuchlichsten Oberflächen ist, die Endbenutzer zur Manipulation von UI-Inhalten verwenden, werden sie auch verschiedene andere Eingabemodalitäten wie Maus und Stift verwenden. In diesen Fällen ist es wichtig, dass die Endbenutzer erkennen, dass Ihre Benutzeroberfläche dynamisch auf ihre Eingaben reagiert, unabhängig davon, welche Eingabemodalität sie verwenden. Sie sollten sich der verschiedenen Eingabemodalitäten bewusst sein, wenn Sie eingabegesteuerte Bewegungserlebnisse gestalten.

## <a name="different-input-driven-motion-experiences"></a>Unterschiedliche eingabegesteuerte Bewegungserfahrungen

Der InputAnimation-Bereich bietet Ihnen verschiedene Möglichkeiten, dynamisch reagierende Bewegungen zu erzeugen. Wie der Rest des Windows-UI-Animationssystems arbeiten auch diese eingabegesteuerten Animationen mit einem unabhängigen Thread, was zum dynamischen Bewegungserlebnis beiträgt. In einigen Fällen, in denen die Erfahrung jedoch auf vorhandene XAML-Steuerelemente und -Komponenten zurückgreift, sind Teile dieser Erfahrungen immer noch UI-Thread-gebunden.

Es gibt drei Haupterlebnisse, die Sie beim Erstellen dynamischer, eingabegesteuerter Bewegungsanimationen erstellen:

1. Erweitern vorhandener ScrollView-Erfahrungen – Erweitern Sie die Position eines XAML ScrollViewers, um dynamische Animationen zu ermöglichen.
1. Durch die Zeigerpositions gesteuerte Erlebnisse – Nutzen Sie die Position eines Cursors auf einem Hit-getesteten UIElement, um dynamische Animationen zu erzeugen.
1. Individuelle Manipulationserfahrungen mit InteractionTracker - Erstellen Sie mit InteractionTracker eine vollständig angepasste, Off-Thread-Manipulationserfahrung (z. B. Scrollen/Zoomen).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Erweitern vorhandener ScrollViewer-Erfahrungen

Eine der häufigsten Möglichkeiten, dynamischere Erlebnisse zu schaffen, ist es, auf einem bestehenden XAML-ScrollViewer-Steuerelement aufzubauen. In diesen Situationen können Sie die Scroll-Position eines ScrollViewer nutzen, um zusätzliche UI-Komponenten zu erstellen, die ein einfaches Scrollerlebnis dynamischer machen. Einige Beispiele sind Sticky/Shy Header und Parallax.

![Listenansicht mit Parallax](images/animation/parallax.gif)

![Ein Shy-Header](images/animation/shy-header.gif)

Bei der Schaffung dieser Art von Erfahrungen gibt es eine allgemeine Formel:

1. Rufen Sie das ScrollManipulationPropertySet aus dem XAML ScrollViewer auf, den Sie animieren möchten.
    - Wird über die ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement element)-API durchgeführt
    - Gibt ein CompositionPropertySet mit einer **Translation**-Eigenschaft zurück
1. Erstellen Sie eine Composition ExpressionAnimation mit einer Gleichung, die auf die Translation-Eigenschaft verweist.
1. Starten Sie die Animation für die Eigenschaft eines CompositionObjects.

Weitere Informationen zum Erstellen dieser Erfahrungen finden Sie unter [Erweitern vorhandener ScrollViewer-Erfahrungen](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Durch die Zeigerposition gesteuerte Erlebnisse

Eine weitere häufige dynamische Erfahrung mit Eingaben ist es, eine Animation basierend auf der Position eines Zeigers wie z. B. eines Mauscursors zu steuern. In diesen Situationen nutzen Entwickler die Position eines Cursors beim Hit-Testen in einem XAML-UIElement, was Erfahrungen wie Spotlight-Reveal ermöglicht.

![Zeiger-Spotlight-Beispiel](images/animation/spotlight-reveal.gif)

![Zeiger-Rotations-Beispiel](images/animation/pointer-rotate.gif)

Bei der Schaffung dieser Art von Erfahrungen gibt es eine allgemeine Formel:

1. Rufen Sie das PointerPositionPropertySet von einem XAML-UIElement auf, von dem Sie de Cursorposition beim Hit-Testen wissen möchten.
    - Wird über die ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element)-API durchgeführt
    - Gibt ein CompositionPropertySet mit einer **Position**-Eigenschaft zurück
1. Erstellen Sie eine CompositionExpressionAnimation mit einer Gleichung, die auf die Position-Eigenschaft verweist.
1. Starten Sie die Animation für die Eigenschaft eines CompositionObjects.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Angepasste Manipulation mit InteractionTracker

Eine der Herausforderungen bei der Verwendung eines XAML-ScrollViewer ist, dass er an den UI-Thread gebunden ist. Daher kann das Scrollen und Zoomen oft verzögern und stottern, wenn der UI-Thread beschäftigt ist, und zu einem unattraktiven Erlebnis führt. Darüber hinaus ist es nicht möglich, viele Aspekte des ScrollViewer-Erlebnisses anzupassen. InteractionTracker wurde entwickelt, um beide Probleme zu lösen, indem eine Reihe von Bausteinen zur Verfügung gestellt wird, um benutzerdefinierte Manipulationserfahrungen zu erstellen, die in einem unabhängigen Thread ausgeführt wird.

![Beispiel für 3D-Interaktionen](images/animation/interactions-3d.gif)

![Beispiel zum Ziehen zum Animieren](images/animation/pull-to-animate.gif)

Bei der Erstellung von Erfahrungen mit InteractionTracker gibt es eine allgemeine Formel:

1. Erstellen Sie Ihr InteractionTracker-Objekt und definieren Sie dessen Eigenschaften.
1. Erstellen Sie VisualInteractionSources für jedes CompositionVisual, das Eingaben für InteractionTracker erfassen soll.
1. Erstellen Sie eine Composition ExpressionAnimation mit einer Gleichung, die auf die Position-Eigenschaft von InteractionTracker verweist.
1. Starten Sie die Animation für eine Composition Visual-Eigenschaft, die von InteractionTracker gesteuert werden soll.
