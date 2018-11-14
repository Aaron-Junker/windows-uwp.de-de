---
author: jwmsft
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Visuelle Ebene
description: Die Windows.UI.Composition-API ermöglicht den Zugriff auf die Kompositionsebene zwischen der Frameworkebene (XAML) und der Grafikebene (DirectX).
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2dd8c53dad735cf1094410bf97a81f6b0247bdc7
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "6650033"
---
# <a name="visual-layer"></a>Visuelle Ebene

Die visuelle Ebene bietet eine hohe Leistung, Speichermodus-API für Grafiken, Effekte und Animationen und ist die Grundlage für alle UI-Elemente für Windows-Geräte.Sie definieren die UI auf einfache Weise und die visuelle Ebene nutzt Hardwarebeschleunigungsgrafiken, um sicherzustellen, dass Ihre Inhalte, Effekte und Animationen reibungslos, störungsfrei und unabhängig vom UI-Thread der App gerendert werden.

Wichtige Highlights:

* Vertraute WinRT-APIs
* Entwickelt für eine dynamischere Benutzeroberfläche und Interaktionen
* Konzepte an Entwurfstools ausgerichtet
* Lineare Skalierbarkeit ohne plötzliche Leistungsprobleme

Die Windows-UWP-Apps verwenden bereits die visuelle Ebene über eines der UI-Frameworks. Sie können die visuelle Ebene auch direkt mit wenig Aufwand zum benutzerdefinierten Rendern, für Effekte und Animationen nutzen.

![Anordnung des UI-Framework: Die Frameworkebene (Windows.UI.XAML) basiert auf der visuellen Ebene (Windows.UI.Composition), die wiederum auf der Grafikebene (DirectX) basiert.](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>Was befindet sich in der visuellen Ebene?

Die wichtigsten Funktionen der visuellen Ebene sind:

1. **Inhalt**: Einfache Zusammensetzung des benutzerdefinierten gezeichneten Inhalts
1. **Effekte**: UI-Effektsystem in Echtzeit, dessen Effekte animiert, verkettet und angepasst werden können
1. **Animationen**: Ausdrucksstarke, Framework-agnostische Animationen, die unabhängig vom UI-Thread ausgeführt werden

### <a name="content"></a>Inhalt

Inhalt wird vom Animations- und Effektsystem mit visuellen Elementen für die Verwendung gehostet, transformiert und zur Verfügung gestellt. An der Basis der Klassenhierarchie befindet sich die [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Klasse, ein leichter Thread-Agile-Proxy im App-Prozess zum visuellen Zustand im Kompositor. Unterklassen der Visual [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) zulassen für Kinder erstellen Strukturen von visuellen Elementen und [**spritevisual-Element**](https://msdn.microsoft.com/library/windows/apps/Mt589433) , das enthält Inhalte enthalten und können mit Volltonfarben, benutzerdefinierten gezeichneten Inhalten oder visuellen Effekten gezeichnet werden. Zusammen bilden diese Visual-Typen die visuelle Struktur für die 2D-UI und stützen die meisten sichtbaren XAML-FrameworkElements.

Weitere Informationen finden Sie in der Übersicht [Visuelle Kompositionseffekte](composition-visual-tree.md).

### <a name="effects"></a>Effekte

Das Effektsystem in der visuellen Ebene ermöglicht Ihnen das Anwenden einer Kette von Filter- und Transparenzeffekten auf ein visuelles Element oder eine Struktur der visuellen Elemente. Dies ist ein UI-Effektsystem und nicht zu verwechseln mit Bild- und Medieneffekten. Effekte funktionieren in Verbindung mit dem Animationssystem und ermöglichen, dass Benutzer nahtlos und dynamisch Animationen von Effekteigenschaften erzielen können, die unabhängig vom UI-Thread gerendert wurden. Effekte in der visuellen Ebene bieten kreative Bausteine, die kombiniert und animiert werden können, um speziell angepasste und interaktive Benutzeroberflächen zu erstellen.

Neben animierbaren Effektketten unterstützt die visuelle Ebene auch ein Lichtmodell, mit dem visuelle Elemente Materialeigenschaften durch die Reaktion auf animierbare Lichter nachahmen können. Visuelle Elemente können auch Schatten werfen. Licht und Schatten können kombiniert werden, um eine Wahrnehmung von Tiefe und Realismus zu erstellen.

Weitere Informationen finden Sie in der Übersicht [Kompositionseffekte](composition-effects.md).

### <a name="animations"></a>Animationen

Das Animationssystem in der visuellen Ebene ermöglicht das Verschieben von visuellen Elementen, Animieren von Effekten und Erstellen von Transformationen, Clips und anderen Eigenschaften.Es ist ein Framework-agnostisches System, das durchgehend im Hinblick auf hohe Leistung entwickelt wurde.Es wird unabhängig vom UI-Thread ausgeführt, um Gleichmäßigkeit und Skalierbarkeit sicherzustellen.Während es Ihnen ermöglicht, mithilfe vertrauter KeyFrame-Animationen Eigenschaften im Laufe der Zeit zu ändern, ermöglicht es auch das Aufstellen von mathematischen Beziehungen zwischen unterschiedlichen Eigenschaften, z.B. Benutzereingaben, mit denen Sie direkt nahtlos choreographierte Umgebungen erstellen können.

Weitere Informationen finden Sie in der Übersicht [Kompositionsanimationen](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Arbeit mit der XAML-UWP-App

Sie können auf ein visuelles Element, das über das XAML-Framework erstellt wurde, zugreifen und ein sichtbares FrameworkElement sichern, indem Sie die [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976)-Klasse in [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908) verwenden. Beachten Sie, dass einige Einschränkungen der Anpassung für visuelle Elemente gelten, die über das Framework für Sie erstellt wurden. Dies liegt daran, dass das Framework Offsets, Transformationen und Lebensdauer-Zeitspannen verwaltet. Sie können jedoch Ihre eigenen visuellen Elemente erstellen und sie einem vorhandenen XAML-Element über ElementCompositionPreview anhängen. Sie können sie auch einem vorhandenen ContainerVisual an einer beliebigen Stelle in der visuellen Struktur hinzufügen.

Weitere Informationen finden Sie in der Übersicht [Benutzung der visuellen Ebene mit XAML](using-the-visual-layer-with-xaml.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [**Vollständige Referenzdokumentation für die API**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
* Erweiterte UI und Kompositionsbeispiele in dem [WindowsUIDevLabs-GitHub](https://github.com/microsoft/windowsuidevlabs).
* [Windows.UI.Composition-Beispielgalerie](https://aka.ms/winuiapp)
* [@windowsui Twitter-Feed ](https://twitter.com/windowsui)
* Lesen Sie den MSDN-Artikel von Kenny Kerr zu dieser API: [Grafiken und Animationen – Windows Composition wird 10](https://msdn.microsoft.com/magazine/mt590968)
