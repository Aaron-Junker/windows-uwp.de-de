---
title: Modernisieren Sie Ihre desktop-app mit der visuellen Ebene
description: Verwenden Sie der visuellen Ebene, um die Benutzeroberfläche der .NET oder Win32-desktop-app zu verbessern.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215174"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Mithilfe der visuellen Ebene in desktop-apps

Sie können jetzt UWP-APIs in nicht-UWP-desktop-Anwendungen verwenden, zur Verbesserung der Aussehens, Verhaltens, und Funktionalität von Ihrer Windows Forms, WPF und C++ Win32-Anwendungen, und nutzen Sie die neuesten Windows 10-Benutzeroberflächen-Features, die nur über die UWP verfügbar sind.

In vielen Fällen können Sie [XAML-Inseln](xaml-islands.md) moderne XAML-Steuerelemente zu Ihrer app hinzufügen. Wenn Sie möchten benutzerdefinierte Anwendungen zu schaffen, die über die integrierten Steuerelemente hinausgehen, können Sie jedoch die visuelle Ebene APIs zugreifen.

Die visuelle Ebene bietet eine hohe Leistung, beibehalten Mode-API für Grafiken, Animationen und Effekte. Es ist die Grundlage für die Benutzeroberfläche auf Windows 10-Geräten. UWP XAML-Steuerelemente werden auf der visuellen Ebene erstellt und ermöglicht, viele Aspekte der [Fluent-Entwurfssystem](/windows/uwp/design/fluent-design-system/index), z. B. Licht, Tiefe, Bewegung, Material und Skalierung.

![Benutzeroberfläche erstellt wurde, mit der visuellen Ebene](images/visual-layer-interop/pull-to-animate.gif)

> _Benutzeroberfläche erstellt wurde, mit der visuellen Ebene_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Erstellen einer ansprechenden Benutzeroberfläche in einer beliebigen Windows-app

Die visuelle Ebene können Sie ansprechende Benutzeroberflächen zu erstellen, indem einfache zusammensetzen von benutzerdefinierten gezeichneten Inhalt (Visuals) und leistungsstarke Animationen, Auswirkungen und Bearbeitungen für diese Objekte in Ihrer Anwendung anwenden. Die visuelle Ebene ersetzen keine vorhandenen Benutzeroberflächenframework; Stattdessen ist es eine wertvolle Ergänzung für diese Frameworks.

Sie können die visuelle Ebene verwenden, um Ihre Anwendung eine eindeutige Erscheinungsbild verleihen, und eine Identität, die von anderen Anwendungen festgelegt. Darüber hinaus wird die Fluent-Entwurfsprinzipien, die entwickelt wurden, um Ihre Anwendungen einfacher zu verwenden, machen interaktionslevel Benutzer zeichnen können. Angenommen, können es Sie zum Erstellen von visuellen Hinweise und animierten Bildschirmübergänge, die Beziehungen zwischen Elementen auf dem Bildschirm anzuzeigen.

## <a name="visual-layer-features"></a>Features der visuellen Ebene

### <a name="brushes"></a>Pinsel

[Kompositionspinsel](/windows/uwp/composition/composition-brushes) können Sie die UI-Objekte mit Volltonfarben, Farbverläufen, Bildern, Videos, komplexe Effekte und mehr zeichnen.

![Kein Ei mit Material Ersteller erstellt wurden](images/visual-layer-interop/egg.gif)

> _Kein Ei erstellt, mit der [Material-Ersteller-Demo-app](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Effekte

[Auswirkungen der Komposition](/windows/uwp/composition/composition-effects) gehören Licht, Schattenkopien und eine Liste der Filtereffekte. Sie können werden animiert, angepasst, verkettet, und direkt auf visuelle Elemente angewendet. Die SceneLightingEffect kann mit Komposition Beleuchtung Atmosphäre, Tiefe und Materialien für Erstellung kombiniert werden.

![Lichter und Materialien](images/visual-layer-interop/light-interop.gif)

> _Lichter und Material veranschaulicht werden, der [Windows UI-Zusammenstellung Beispielkatalog](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animationen

[Kompositionsanimationen](/windows/uwp/composition/composition-animation) direkt in der Compositor-Prozess, unabhängig von der UI-Thread ausführen. Dadurch wird sichergestellt Einfachheit und Skalierung, sodass Sie eine große Anzahl von gleichzeitigen, explizite Animationen ausführen können. Zusätzlich zur vertrauten KeyFrame-Animationen auf Änderungen der Laufwerk-Eigenschaften im Laufe der Zeit können Sie Ausdrücke verwenden, zum Einrichten von mathematischer Beziehungen zwischen unterschiedlichen Eigenschaften, einschließlich der Benutzereingabe. Eingabe testgesteuerte-Animationen können Sie die Benutzeroberfläche zu erstellen, dynamisch und auch auf Benutzereingaben reagiert, was zu höheren benutzerbindung führen kann.

![Benutzeroberfläche erstellt wurde, mit der visuellen Ebene](images/visual-layer-interop/swipe-scroller.gif)

> _Während der Übertragung in veranschaulicht die [Windows UI-Zusammenstellung Beispielkatalog](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Behalten Sie Ihre vorhandene Codebasis und inkrementell übernehmen

Der Code in Ihre vorhandenen Anwendungen stellt eine erhebliche Investition, die Sie nicht verlieren möchten. Sie migrieren _Inseln_ von Inhalten in der visuellen Ebene verwenden, und verwenden den Rest der Benutzeroberfläche in der vorhandenen Framework. Dies bedeutet, dass Sie wichtige Updates und Verbesserungen an der Benutzeroberfläche Ihrer Anwendung vornehmen können ohne umfangreiche Änderungen an den vorhandenen Code vornehmen Basis.

## <a name="samples-and-tutorials"></a>Beispiele und Lernprogramme

Erfahren Sie, wie Sie mit der visuellen Ebene in Ihren Anwendungen mit unseren beispielapps und auszuprobieren. Diese Beispiele und Lernprogramme helfen, die Ihnen erste Schritte mit der visuellen Ebene und zeigen Ihnen, wie Features funktionieren.

### <a name="win32"></a>Win32

- [Mithilfe der visuellen Ebene mit Win32](using-the-visual-layer-with-win32.md) Tutorial
  - [Hello-Kompositions-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello-Vektoren-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Virtuelle Oberflächen-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Bildschirm der Capture-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [Windows Forms mit der visuellen Ebene](using-the-visual-layer-with-windows-forms.md) Tutorial
  - [Hello-Kompositions-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Visual-Layer-integrationsbeispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [Mithilfe der visuellen Ebene mit WPF](using-the-visual-layer-with-wpf.md) Tutorial
  - [Hello-Kompositions-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Visual-Layer-integrationsbeispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Bildschirm der Capture-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Einschränkungen

Während viele Funktionen der visuellen Ebene funktionieren, wenn in einer desktop-Anwendung gehostet werden soll, wie in einer UWP-app ausführen, müssen einige Features Einschränkungen. Hier sind einige der Einschränkungen zu beachten:

- Auswirkungen Ketten basieren auf [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) Beschreibungen zu den Auswirkungen. Die [Win2D-NuGet-Paket](https://www.nuget.org/packages/Win2D.uwp) wird nicht in desktop-Anwendungen unterstützt, damit Sie von neu kompilieren müssen die [Quellcode](https://github.com/Microsoft/Win2D).
- Um die Treffertests, müssen Sie dazu, dass Grenzen Berechnungen Durchlaufen der visuellen Struktur selbst. Dies ist identisch mit der visuellen Ebene auf UWP, mit der Ausnahme besteht in diesem Fall keine XAML-Element, das Sie ganz einfach für den Treffertest binden können.
- Der visuellen Ebene muss ein primitiver Typ zum Rendern von Text nicht.
- Wenn zwei verschiedene UI-Technologien zusammen verwendet werden, z. B. WPF und der visuellen Ebene, sie sind dafür verantwortlich, ihre eigenen Pixel auf dem Bildschirm zeichnen jedes, und sie können keine Pixel aufweisen. Daher wird der Inhalt der visuellen Ebene immer auf andere Inhalte der Benutzeroberfläche gerendert. (Dies wird als bezeichnet die _Airspace_ Problem.) Möglicherweise müssen Sie zusätzlichen Code geschrieben werden muss, und testen, um sicherzustellen, dass Ihre Inhalte der visuellen Ebene mit dem UI-Host und der Größe der keine anderen Inhalten verdeckt.
- In einer desktop-Anwendung gehosteten Inhalt nicht automatisch ändern der Größe oder skalieren Sie für die DPI-Wert. Möglicherweise zusätzliche Schritte erforderlich, um sicherzustellen, dass die DPI-Änderungen in Ihren Inhalt behandelt. (Siehe die spezifische Plattform-Lernprogramme für Weitere Informationen.)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Visuelle Ebene](/windows/uwp/composition/visual-layer)
- [Visuelle Komposition](/windows/uwp/composition/composition-visual-tree)
- [Kompositionspinsel](/windows/uwp/composition/composition-brushes)
- [Auswirkungen der Komposition](/windows/uwp/composition/composition-effects)
- [Kompositionsanimationen](/windows/uwp/composition/composition-animation)

API-Referenz

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)