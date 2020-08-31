---
title: Eingabegesteuerte Animationen
description: Erfahren Sie mehr über die inputanimation-API und wie Sie Eingabe gesteuerte Animationen verwenden, um auf Ihrer App-Benutzeroberfläche dynamisch reaktionsfähig zu machen.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: 0f9abd902e39b645f27b7a0f5d521097ca4aff8a
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054360"
---
# <a name="input-driven-animations"></a>Eingabegesteuerte Animationen

Dieser Artikel bietet eine Einführung in die inputanimation-API und empfiehlt die Verwendung dieser Animations Typen in der Benutzeroberfläche.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie mit den Konzepten vertraut sind, die in den folgenden Artikeln erläutert werden:

- [Beziehungs basierte Animationen](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Reibungslose Bewegung von Benutzerinteraktionen

In der Sprache "fließend Design" ist die Interaktion zwischen Endbenutzern und apps von größter Wichtigkeit. Apps müssen nicht nur den Teil sehen, sondern auch natürlich und dynamisch auf die Benutzer reagieren, die mit ihnen interagieren. Dies bedeutet, wenn ein Finger auf dem Bildschirm platziert wird, sollte die Benutzeroberfläche ordnungsgemäß auf veränderliche Grad der Eingabe reagieren. der Bildlauf sollte glatt sein und auf dem Bildschirm auf einen Finger schwenken zeigen.

Die Erstellung einer Benutzeroberfläche, die dynamisch und fließend auf Benutzereingaben reagiert, führt zu einer höheren Benutzer Bindung, die jetzt nicht nur in Ordnung ist, sondern sich gut und natürlich bewährt, wenn Benutzer mit ihren unterschiedlichen Benutzeroberflächen Erfahrungen interagieren. Dies ermöglicht es Endbenutzern, eine einfachere Verbindung mit Ihrer Anwendung herzustellen

## <a name="expanding-past-just-touch"></a>Erweitern der früheren Interaktion

Obwohl die Berührungs Weise eine der gängigeren Schnittstellen ist, die Endbenutzer verwenden, um UI-Inhalte zu bearbeiten, verwenden Sie auch verschiedene andere Eingabemethoden, z. b. Maus und Stift. In diesen Fällen ist es wichtig, dass Endbenutzer erkennen, dass Ihre Benutzeroberfläche dynamisch auf Ihre Eingaben antwortet, unabhängig davon, welche Eingabe Modalität Sie verwenden möchten. Sie sollten die verschiedenen Eingabe Modalitäten erkennen, wenn Sie eine Eingabe gesteuerte Bewegungserfahrung entwerfen.

## <a name="different-input-driven-motion-experiences"></a>Unterschiedliche Eingabe gesteuerte Bewegungsmöglichkeiten

Der inputanimation-Bereich bietet verschiedene unterschiedliche Möglichkeiten zum Erstellen dynamisch Reaktions ender Bewegungen. Ebenso wie der Rest des Windows-UI-Animations Systems arbeiten diese Eingabe gesteuerten Animationen in einem unabhängigen Thread, der zur dynamischen Bewegungs Darstellung beiträgt. In einigen Fällen, in denen die Benutzeroberfläche vorhandene XAML-Steuerelemente und-Komponenten nutzt, sind Teile dieser Oberflächen weiterhin UI-Thread gebunden.

Es gibt drei grundlegende Benutzeroberflächen, die Sie beim Erstellen dynamischer Eingabe gesteuerte Bewegungs Animationen erstellen können:

1. Verbessern vorhandener ScrollView-Funktionen – aktivieren Sie die Position eines XAML ScrollViewer, um dynamische Animations Funktionen zu fördern.
1. Zeiger Positions gesteuerte Oberflächen – verwenden Sie die Position eines Cursors auf einem Treffer geprüften UIElement, um dynamische Animations Funktionen zu fördern.
1. Benutzerdefinierte Manipulations Erfahrung mit interaktiontracker – erstellen Sie eine vollständig angepasste, nicht Thread gesteuerte Bearbeitungsumgebung mit interaktiontracker (z. b. ein Scroll-/Zoom-Canvas).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Verbessern vorhandener ScrollViewer-Erfahrungen

Eine der gängigsten Möglichkeiten, dynamischere Umgebungen zu erstellen, besteht darin, ein vorhandenes XAML ScrollViewer-Steuerelement zu erstellen. In diesen Fällen nutzen Sie die Scrollposition eines ScrollViewer-Steuerelemente, um zusätzliche Benutzeroberflächen Komponenten zu erstellen, die eine einfache Bild Lauf Darstellung dynamischer gestalten. Beispiele hierfür sind Kurznotiz/scheue Header und eine Element-/Warnung.

![Listenansicht mit "initiallax"](images/animation/parallax.gif)

![Ein Scheuer Header](images/animation/shy-header.gif)

Beim Erstellen dieser Arten von Erfahrungen gibt es eine allgemeine Formel, die befolgt werden muss:

1. Greifen Sie auf das scrollmanipulationpropertyset aus dem XAML ScrollViewer zu, um eine Animation zu steuern.
    - Durchgeführt über die elementcompositionpreview. getscrollviewermanipulationpropertyset (UIElement-Element)-API
    - Gibt ein compositionpropertyset mit einer Eigenschaft namens **Translation** zurück.
1. Erstellen Sie eine Komposition expressionanimation mit einer Gleichung, die auf die Translation-Eigenschaft verweist.
1. Starten Sie die Animation für die-Eigenschaft eines compositionobject.

Weitere Informationen zum Entwickeln dieser Erfahrungen finden Sie unter [verbessern vorhandener ScrollViewer-Erfahrungen](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Zeiger Positions gesteuerte Oberflächen

Eine andere häufige dynamische Darstellung mit Eingaben besteht darin, eine Animation auf der Grundlage der Position eines Zeigers (z. b. eines Mauszeigers) zu steuern. In diesen Fällen nutzen Entwickler die Position eines Cursors, wenn der Treffer in einem XAML-UIElement getestet wird, das das Erstellen von Erlebnissen wie Spotlight ermöglicht.

![Beispiel für Zeiger Spotlight](images/animation/spotlight-reveal.gif)

![Beispiel für Zeiger Drehung](images/animation/pointer-rotate.gif)

Beim Erstellen dieser Arten von Erfahrungen gibt es eine allgemeine Formel, die befolgt werden muss:

1. Greifen Sie in einem XAML-UIElement auf das pointerpositionpropertyset zu, um die Cursorposition bei Treffer Tests zu kennen.
    - Abgeschlossen über die elementcompositionpreview. getpointerpositionpropertyset (UIElement-Element)-API
    - Gibt ein compositionpropertyset zurück, das eine Eigenschaft namens **Position** enthält.
1. Erstellen Sie eine compositionexpressionanimation mit einer Gleichung, die auf die Positions Eigenschaft verweist.
1. Starten Sie die Animation für die-Eigenschaft eines compositionobject.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Benutzerdefinierte Manipulations Erfahrung mit interaktiontracker

Eine der Herausforderungen bei der Verwendung eines XAML-ScrollViewers besteht darin, dass Sie an den UI-Thread gebunden ist. Folglich kann das Scrollen und Zoomen häufig verzögert werden und Jitter, wenn der UI-Thread ausgelastet wird und zu einer nicht ansprechenden Benutzeroberfläche führt. Außerdem ist es nicht möglich, viele Aspekte der ScrollViewer-Darstellung anzupassen. Interaktiontracker wurde erstellt, um beide Probleme zu lösen, indem eine Reihe von Bausteinen bereitgestellt werden, um benutzerdefinierte Bearbeitungs Umgebungen zu erstellen, die in einem unabhängigen Thread ausgeführt werden.

![Beispiel für 3D-Interaktionen](images/animation/interactions-3d.gif)

![Pull to animieren (Beispiel)](images/animation/pull-to-animate.gif)

Beim Erstellen von Erfahrungen mit interaktiontracker gibt es eine allgemeine Formel, die befolgt werden muss:

1. Erstellen Sie das interaktiontracker-Objekt, und definieren Sie seine Eigenschaften.
1. Erstellen Sie visualinteraktionsources für ein beliebiges compositionvisual, das Eingaben erfassen soll, die von interaktiontracker verwendet werden sollen.
1. Erstellen Sie eine Komposition expressionanimation mit einer Gleichung, die auf die Positions Eigenschaft von interaktiontracker verweist.
1. Starten Sie die Animation für die Eigenschaft einer Kompositions Visualisierung, die Sie von interaktiontracker gesteuert werden möchten.
