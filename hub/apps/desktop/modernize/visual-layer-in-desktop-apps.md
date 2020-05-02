---
title: Modernisieren deiner Desktop-App über die visuelle Ebene
description: Verwende die visuelle Ebene, um die Benutzeroberfläche deiner .NET- oder Win32-Desktop-App zu verbessern.
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215174"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Verwenden der visuellen Ebene in Desktop-Apps

Du kannst die UWP-APIs jetzt auch in anderen Desktopanwendungen als UWP-Apps verwenden, um Aussehen, Handhabung und Funktionalität deiner WPF-, Windows Forms- und C++-Win32-Anwendungen zu verbessern, und die aktuellen Features der Windows 10-Benutzeroberfläche nutzen, die nur per UWP verfügbar sind.

In vielen Szenarien kannst du [XAML Islands](xaml-islands.md) verwenden, um deiner App moderne XAML-Steuerelemente hinzuzufügen. Wenn du jedoch benutzerdefinierte Umgebungen über die integrierten Steuerelemente hinaus erstellen musst, kannst du die APIs der visuellen Ebene verwenden.

Die visuelle Ebene bietet eine leistungsstarke Speichermodus-API für Grafiken, Effekte und Animationen. Sie bildet die Grundlage für die Benutzeroberfläche auf Windows 10-Geräten. UWP-XAML-Steuerelemente basieren auf der visuellen Ebene und ermöglichen zahlreiche Aspekte des [Fluent Design-Systems](/windows/uwp/design/fluent-design-system/index), z. B. Licht, Tiefe, Bewegung, Material und Skalierung.

![Mit der visuellen Ebene erstellte Benutzeroberfläche](images/visual-layer-interop/pull-to-animate.gif)

> _Mit der visuellen Ebene erstellte Benutzeroberfläche_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Erstellen einer visuell ansprechenden Benutzeroberfläche in einer Windows-App

Die visuelle Ebene ermöglicht dir, auf einfache Weise über selbst gezeichnete Inhalte (visuelle Elemente) ansprechende Benutzeroberflächen zu erstellen und aufwendige Animationen, Effekte und Anpassungen auf diese Objekte in der Anwendung anzuwenden. Die visuelle Ebene ersetzt dabei kein vorhandenes Benutzeroberflächenframework, sondern sie stellt eine wertvolle Ergänzung dieser Frameworks dar.

Du kannst die visuelle Ebene nutzen, um deiner Anwendung ein einzigartiges Aussehen mit einer eigenen Bedienung zu verleihen und damit eine persönliche Identität zu schaffen, die sie von anderen Anwendungen abhebt. Außerdem werden Fluent Design-Prinzipien einbezogen, die speziell entwickelt wurden, um die Verwendung deiner Anwendungen zu vereinfachen und die Benutzer damit stärker zu binden. Beispielsweise kannst du damit visuelle Hinweise und animierte Bildschirmübergänge erzeugen, die die Beziehungen zwischen Elementen auf dem Bildschirm verdeutlichen.

## <a name="visual-layer-features"></a>Funktionen der visuellen Ebene

### <a name="brushes"></a>Pinsel

Mit [Kompositionspinseln](/windows/uwp/composition/composition-brushes) kannst du Benutzeroberflächenobjekte mit Volltonfarben, Farbverläufen, Bildern, Videos, komplexen Effekten und Ähnlichem zeichnen.

![Ein mit Material Creator erstelltes Ei](images/visual-layer-interop/egg.gif)

> _Ein mit der [Material Creator-Beispiel-App](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator) erstelltes Ei_

### <a name="effects"></a>Effekte

Zu den [Kompositionseffekten](/windows/uwp/composition/composition-effects) zählen Licht und Schatten sowie eine Liste mit Filtereffekten. Sie können animiert, angepasst und verkettet und dann direkt auf visuelle Elemente angewandt werden. Der SceneLightingEffect kann mit der Kompositionsbeleuchtung kombiniert werden, um mehr Atmosphäre, Tiefe und Materialien zu schaffen.

![Licht und Material](images/visual-layer-interop/light-interop.gif)

> _Licht und Material – demonstriert im [Beispielkatalog zur Windows-Benutzeroberflächenerstellung](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animationen

[Kompositionsanimationen](/windows/uwp/composition/composition-animation) werden unabhängig vom Benutzeroberflächenthread direkt im Compositor-Prozess ausgeführt. Dadurch werden Ruckelfreiheit und Skalierung sichergestellt, sodass du eine große Anzahl gleichzeitiger, aufwendiger Animationen ausführen kannst. Zusätzlich zu den vertrauten KeyFrame-Animationen zum Steuern von Eigenschaftsänderungen über einen Zeitraum kannst du Ausdrücke verwenden, um mathematische Beziehungen zwischen verschiedenen Eigenschaften (einschließlich Benutzereingaben) einzurichten. Mit eingabegesteuerten Animationen kannst du eine Benutzeroberfläche erstellen, die dynamisch und fließend auf Benutzereingaben reagiert und damit die Benutzerbindung steigern kann.

![Mit der visuellen Ebene erstellte Benutzeroberfläche](images/visual-layer-interop/swipe-scroller.gif)

> _Bewegung – demonstriert im [Beispielkatalog zur Windows-Benutzeroberflächenerstellung](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Beibehalten der vorhandenen Codebasis mit Übernahme in einzelnen Schritten

Der Code in deinen vorhandenen Anwendungen stellt eine bedeutende Investition dar, die du natürlich nicht verlieren möchtest. Du kannst _Inseln_ von Inhalten zur visuellen Ebene migrieren und den Rest der Benutzeroberfläche im vorhandenen Framework beibehalten. So kannst du erhebliche Updates und Verbesserungen an der Benutzeroberfläche deiner Anwendung vornehmen, ohne die vorhandene Codebasis umfassend ändern zu müssen.

## <a name="samples-and-tutorials"></a>Beispiele und Tutorials

Erfahre durch Experimentieren mit unseren Beispielen, wie du die visuelle Ebene in deinen Anwendungen verwenden kannst. Diese Beispiele und Tutorials helfen dir bei den ersten Schritten mit der visuellen Ebene und zeigen dir, wie die Features funktionieren.

### <a name="win32"></a>Win32

- Tutorial zum [Verwenden der visuellen Ebene mit Win32](using-the-visual-layer-with-win32.md)
  - [Beispiel „HelloComposition“](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Beispiel „HelloVectors“](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Beispiel für virtuelle Oberflächen](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Beispiel für die Bildschirmerfassung](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- Tutorial zum [Verwenden der visuellen Ebene mit Windows Forms](using-the-visual-layer-with-windows-forms.md)
  - [Beispiel „HelloComposition“](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Beispiel für die Integration der visuellen Ebene](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- Tutorial zum [Verwenden der visuellen Ebene mit WPF](using-the-visual-layer-with-wpf.md)
  - [Beispiel „HelloComposition“](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Beispiel für die Integration der visuellen Ebene](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Beispiel für die Bildschirmerfassung](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Einschränkungen

Obwohl viele Funktionen der visuellen Ebene beim Hosten in einer Desktopanwendung genauso funktionieren wie in einer UWP-App, gelten für einige Features Einschränkungen. Im Folgenden findest du einige Einschränkungen, die du kennen solltest:

- Effektketten sind für Effektbeschreibungen auf [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) angewiesen. Das [Win2D-NuGet-Paket](https://www.nuget.org/packages/Win2D.uwp) wird in Desktopanwendungen nicht unterstützt, daher musst du es aus dem [Quellcode](https://github.com/Microsoft/Win2D) neu kompilieren.
- Für Treffertests musst du Grenzwertberechnungen durchführen, indem du die visuelle Struktur selbst durchläufst. Dies entspricht der visuellen Ebene in UWP, außer dass es in diesem Fall kein XAML-Element gibt, für das du für Treffertests einfach eine Bindung herstellen kannst.
- Die visuelle Ebene bietet keinen primitiven Datentyp zum Rendern von Text.
- Wenn zwei verschiedene Benutzeroberflächentechnologien gemeinsam verwendet werden, z. B. WPF und die visuelle Ebene, sind beide dafür verantwortlich, ihre eigenen Pixel auf dem Bildschirm zu zeichnen – sie können keine Pixel gemeinsam verwenden. Daher wird der Inhalt der visuellen Ebene immer über anderen Benutzeroberflächeninhalten gerendert. (Dies wird als _Airspace_-Problem bezeichnet.) Möglicherweise musst du zusätzlichen Code erstellen und Tests durchführen, um sicherzustellen, dass die Größe des Inhalts deiner visuellen Ebene mit der Hostbenutzeroberfläche geändert wird und keine andere Inhalte verdeckt werden.
- Inhalte, die in einer Desktopanwendung gehostet werden, werden nicht automatisch gemäß dem DPI-Wert in ihrer Größe angepasst oder skaliert. Möglicherweise sind zusätzliche Schritte erforderlich, um sicherzustellen, dass DPI-Änderungen von den Inhalten verarbeitet werden. (Weitere Informationen findest du in den plattformspezifischen Tutorials.)

## <a name="additional-resources"></a>Weitere Ressourcen

- [Visuelle Ebene](/windows/uwp/composition/visual-layer)
- [Visuelle Kompositionselemente](/windows/uwp/composition/composition-visual-tree)
- [Kompositionspinsel](/windows/uwp/composition/composition-brushes)
- [Kompositionseffekte](/windows/uwp/composition/composition-effects)
- [Kompositionsanimationen](/windows/uwp/composition/composition-animation)

API-Referenz

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)