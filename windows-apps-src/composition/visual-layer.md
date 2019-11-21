---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Visuelle Ebene
description: Die Windows.UI.Composition-API ermöglicht den Zugriff auf die Kompositionsebene zwischen der Frameworkebene (XAML) und der Grafikebene (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ac41d461982a39a939e460b7a81b144e5a08fdb3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255526"
---
# <a name="visual-layer"></a>Visuelle Ebene

Die visuelle Ebene bietet eine hohe Leistung, Speichermodus-API für Grafiken, Effekte und Animationen und ist die Grundlage für alle UI-Elemente für Windows-Geräte. Sie definieren die Benutzeroberfläche auf deklarative Weise, und die visuelle Schicht basiert auf der Beschleunigung der Grafikhardware, um sicherzustellen, dass Ihre Inhalte, Effekte und Animationen unabhängig vom UI-Thread der APP nahtlos und problemlos gerendert werden.

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

Inhalt wird vom Animations- und Effektsystem mit visuellen Elementen für die Verwendung gehostet, transformiert und zur Verfügung gestellt. An der Basis der Klassenhierarchie befindet sich die [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual)-Klasse, ein leichter Thread-Agile-Proxy im App-Prozess zum visuellen Zustand im Kompositor. Zu den untergeordneten Klassen von Visual gehört  [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) , damit untergeordnete Elemente Strukturen von visuellen Elementen und [**spritevisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) erstellen können, die Inhalte enthalten und entweder mit voll Tonfarben, mit benutzerdefiniertem gezeichneten Inhalt oder mit visuellen Effekten gezeichnet werden können. Zusammen bilden diese Visual-Typen die visuelle Struktur für die 2D-UI und stützen die meisten sichtbaren XAML-FrameworkElements.

Weitere Informationen finden Sie in der Übersicht [Visuelle Kompositionseffekte](composition-visual-tree.md).

### <a name="effects"></a>Effekte

Das Effektsystem in der visuellen Ebene ermöglicht Ihnen das Anwenden einer Kette von Filter- und Transparenzeffekten auf ein visuelles Element oder eine Struktur der visuellen Elemente. Dies ist ein UI-Effektsystem und nicht zu verwechseln mit Bild- und Medieneffekten. Effekte funktionieren in Verbindung mit dem Animationssystem und ermöglichen, dass Benutzer nahtlos und dynamisch Animationen von Effekteigenschaften erzielen können, die unabhängig vom UI-Thread gerendert wurden. Effekte in der visuellen Ebene bieten kreative Bausteine, die kombiniert und animiert werden können, um speziell angepasste und interaktive Benutzeroberflächen zu erstellen.

Neben animierbaren Effektketten unterstützt die visuelle Ebene auch ein Lichtmodell, mit dem visuelle Elemente Materialeigenschaften durch die Reaktion auf animierbare Lichter nachahmen können. Visuelle Elemente können auch Schatten werfen. Licht und Schatten können kombiniert werden, um eine Wahrnehmung von Tiefe und Realismus zu erstellen.

Weitere Informationen finden Sie in der Übersicht [Kompositionseffekte](composition-effects.md).

### <a name="animations"></a>Animationen

Das Animationssystem in der visuellen Ebene ermöglicht das Verschieben von visuellen Elementen, Animieren von Effekten und Erstellen von Transformationen, Clips und anderen Eigenschaften.  Dabei handelt es sich um ein Framework-agnostisches System, das von Grund auf im Hinblick auf die Leistung entworfen wurde.  Sie wird unabhängig vom UI-Thread ausgeführt, um glätbarkeit und Skalierbarkeit zu gewährleisten.  Obwohl Sie mit Ihnen vertraute Keyframe-Animationen verwenden können, um Eigenschafts Änderungen im Laufe der Zeit zu steuern, können Sie auch mathematische Beziehungen zwischen verschiedenen Eigenschaften einrichten, einschließlich der Benutzereingaben, sodass Sie nahtlose, choreographte Umgebungen direkt erstellen können.

Weitere Informationen finden Sie in der Übersicht [Kompositionsanimationen](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Arbeit mit der XAML-UWP-App

Sie können auf ein visuelles Element, das über das XAML-Framework erstellt wurde, zugreifen und ein sichtbares FrameworkElement sichern, indem Sie die [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)-Klasse in [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting) verwenden. Beachten Sie, dass einige Einschränkungen der Anpassung für visuelle Elemente gelten, die über das Framework für Sie erstellt wurden. Dies liegt daran, dass das Framework Offsets, Transformationen und Lebensdauer-Zeitspannen verwaltet. Sie können jedoch Ihre eigenen visuellen Elemente erstellen und sie einem vorhandenen XAML-Element über ElementCompositionPreview anhängen. Sie können sie auch einem vorhandenen ContainerVisual an einer beliebigen Stelle in der visuellen Struktur hinzufügen.

Weitere Informationen finden Sie in der Übersicht [Benutzung der visuellen Ebene mit XAML](using-the-visual-layer-with-xaml.md).

### <a name="working-with-your-desktop-app"></a>Arbeiten mit ihrer Desktop-App

Mithilfe der visuellen Ebene können Sie das Aussehen, das Gefühl und die Funktionalität Ihrer WPF-, Windows Forms-und C++ Win32-Desktop-Apps verbessern. Sie können Inhalts Inseln migrieren, um die visuelle Schicht zu verwenden, und den Rest der Benutzeroberfläche im vorhandenen Framework beibehalten. Dies bedeutet, dass Sie bedeutende Updates und Verbesserungen an der Benutzeroberfläche der Anwendung vornehmen können, ohne umfassende Änderungen an der vorhandenen Codebasis vornehmen zu müssen.

Weitere Informationen finden Sie unter [Modernize your desktop app using the Visual layer](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps) (Modernisieren Ihrer Desktop-App über die visuelle Ebene).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [**Vollständige Referenz Dokumentation für die API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* Erweiterte UI und Kompositionsbeispiele in dem [WindowsUIDevLabs-GitHub](https://github.com/microsoft/WindowsCompositionSamples).
* [Beispiel Katalog für Windows. UI. Composition](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [Twitter-Feed @windowsui](https://twitter.com/windowsui)
* Lesen Sie den MSDN-Artikel von Kenny Kerr zu dieser API: [Grafiken und Animationen – Windows Composition wird 10](https://msdn.microsoft.com/magazine/mt590968)
