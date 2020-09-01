---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Visuelle Ebene
description: Die Windows.UI.Composition-API ermöglicht den Zugriff auf die Kompositionsebene zwischen der Frameworkebene (XAML) und der Grafikebene (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3cdd7c17bc0d419a7449b366b620a4d5654cccc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160724"
---
# <a name="visual-layer"></a>Visuelle Ebene

Die visuelle Schicht bietet eine leistungsstarke API für den beibehaltenen Modus für Grafiken, Effekte und Animationen und bildet die Grundlage für alle Benutzeroberflächen auf Windows-Geräten.Sie definieren die Benutzeroberfläche auf deklarative Weise, und die visuelle Schicht basiert auf der Beschleunigung der Grafikhardware, um sicherzustellen, dass Ihre Inhalte, Effekte und Animationen unabhängig vom UI-Thread der APP nahtlos und problemlos gerendert werden.

Wichtige Highlights:

* Vertraute WinRT-APIs
* Architektur für eine dynamischere Benutzeroberfläche und Interaktionen
* Konzepte, die mit Entwurfs Tools ausgerichtet sind
* Lineare Skalierbarkeit ohne plötzliche Leistungs Klippen

Ihre Windows UWP-Apps verwenden bereits die visuelle Schicht über eines der Benutzeroberflächen-Frameworks. Sie können die visuelle Schicht auch direkt für benutzerdefiniertes Rendering, Effekte und Animationen nutzen, mit sehr geringem Aufwand.

![UI-Framework-Schichten: die Frameworkebene (Windows. UI. XAML) basiert auf der visuellen Ebene (Windows. UI. Composition), die auf der Grafik Schicht (DirectX) basiert.](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>Was ist in der visuellen Schicht?

Die primären Funktionen der visuellen Ebene sind:

1. **Inhalt**: Lightweight Zusammensetzung of Custom Drawn Content
1. **Effekte**: echt Zeiteffekte der Benutzeroberflächen Effekte, deren Effekte animiert, verkettet und angepasst werden können
1. **Animationen**: ausdrucksstarke, Framework-agnostische Animationen, die unabhängig vom UI-Thread ausgeführt werden

### <a name="content"></a>Inhalt

Der Inhalt wird gehostet, transformiert und zur Verwendung durch das Animations-und Effekte System mithilfe von visuellen Elementen zur Verfügung gestellt. An der Basis der Klassenhierarchie befindet sich die [**visuelle**](/uwp/api/Windows.UI.Composition.Visual) Klasse, ein schlanker Thread-Agile-Proxy im App-Prozess für den visuellen Zustand im Compositor. Zu den untergeordneten Klassen von Visual gehört  [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) , damit untergeordnete Elemente Strukturen von visuellen Elementen und [**spritevisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) erstellen können, die Inhalte enthalten und entweder mit voll Tonfarben, mit benutzerdefiniertem gezeichneten Inhalt oder mit visuellen Effekten gezeichnet werden können. Diese visuellen Typen bilden gemeinsame Struktur der visuellen Struktur für die 2D-Benutzeroberfläche und die meisten sichtbaren XAML-Frameworkelemente.

Weitere Informationen finden Sie unter Übersicht über die [Komposition](composition-visual-tree.md) .

### <a name="effects"></a>Effekte

Mit dem Effects-System in der visuellen Schicht können Sie eine Kette von Filter-und Transparenz Effekten auf ein visuelles Element oder eine visuelle Struktur anwenden. Dabei handelt es sich um ein System mit Benutzeroberflächen Effekten, das nicht mit Bild-und Medien Effekten verwechselt werden kann. Effekte funktionieren in Verbindung mit dem Animationssystem, sodass Benutzer Smooth-und dynamische Animationen von Effekt Eigenschaften erreichen können, die unabhängig vom UI-Thread gerendert werden. Effekte in der visuellen Ebene bieten die kreativen Bausteine, die kombiniert und animiert werden können, um maßgeschneiderte und interaktive Benutzeroberflächen zu erstellen.

Zusätzlich zu den animier baren Effektketten unterstützt die visuelle Schicht auch ein Beleuchtungsmodell, das visuellen Elementen das imitieren von Materialeigenschaften ermöglicht, indem Sie auf animier Bare Leuchten antwortet. Visuelle Elemente können auch Schatten umwandeln. Beleuchtung und Schatten können kombiniert werden, um einen Eindruck von Tiefe und Realismus zu schaffen.

Weitere Informationen finden Sie in der Übersicht [Kompositionseffekte](composition-effects.md).

### <a name="animations"></a>Animationen

Das Animationssystem in der visuellen Ebene ermöglicht Ihnen das Verschieben von visuellen Elementen, das Animieren von Effekten und das Steuern von Transformationen, Clips und anderen Eigenschaften.Dabei handelt es sich um ein Framework-agnostisches System, das von Grund auf im Hinblick auf die Leistung entworfen wurde.Sie wird unabhängig vom UI-Thread ausgeführt, um glätbarkeit und Skalierbarkeit zu gewährleisten.Obwohl Sie mit Ihnen vertraute Keyframe-Animationen verwenden können, um Eigenschafts Änderungen im Laufe der Zeit zu steuern, können Sie auch mathematische Beziehungen zwischen verschiedenen Eigenschaften einrichten, einschließlich der Benutzereingaben, sodass Sie nahtlose, choreographte Umgebungen direkt erstellen können.

Weitere Informationen finden Sie in der Übersicht [Kompositionsanimationen](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Arbeiten mit ihrer XAML-UWP-App

Mithilfe der [**elementcompositionpreview**](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) -Klasse in [**Windows. UI. XAML. Hosting**](/uwp/api/Windows.UI.Xaml.Hosting)können Sie zu einem vom XAML-Framework erstellten visuellen Element gelangen und ein sichtbares FrameworkElement-Element unterstützen. Beachten Sie, dass die für Sie durch das Framework erstellten Visualisierungen einige Beschränkungen für die Anpassung haben. Dies liegt daran, dass das Framework Offsets, Transformationen und Lebens dauern verwaltet. Sie können jedoch eigene Visualisierungen erstellen und Sie über elementcompositionpreview an ein vorhandenes XAML-Element anfügen, oder indem Sie es einer vorhandenen containervisualisierung irgendwo in der visuellen Struktur hinzufügen.

Weitere Informationen finden Sie in der Übersicht über das [Verwenden der visuellen Ebene mit XAML](using-the-visual-layer-with-xaml.md) .

### <a name="working-with-your-desktop-app"></a>Arbeiten mit ihrer Desktop-App

Mithilfe der visuellen Ebene können Sie das Aussehen, das Gefühl und die Funktionalität Ihrer WPF-, Windows Forms-und C++ Win32-Desktop-Apps verbessern. Sie können Inhalts Inseln migrieren, um die visuelle Schicht zu verwenden, und den Rest der Benutzeroberfläche im vorhandenen Framework beibehalten. So kannst du erhebliche Updates und Verbesserungen an der Benutzeroberfläche deiner Anwendung vornehmen, ohne die vorhandene Codebasis umfassend ändern zu müssen.

Weitere Informationen finden Sie unter [Modernize your desktop app using the Visual layer](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps) (Modernisieren Ihrer Desktop-App über die visuelle Ebene).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [**Vollständige Referenz Dokumentation für die API**](/uwp/api/Windows.UI.Composition)
* Erweiterte UI und Kompositionsbeispiele in dem [WindowsUIDevLabs-GitHub](https://github.com/microsoft/WindowsCompositionSamples).
* [Beispiel Katalog für Windows. UI. Composition](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Twitter-Feed ](https://twitter.com/windowsui)
* Lesen Sie den MSDN-Artikel von Kenny Kerr zu dieser API: [Grafiken und Animationen – Windows Composition wird 10](/archive/msdn-magazine/2015/windows-10-special-issue/graphics-and-animation-windows-composition-turns-10)