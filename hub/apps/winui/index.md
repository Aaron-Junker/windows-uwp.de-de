---
title: Windows-UI-Bibliothek (WinUI)
description: WinUI-Bibliotheken für die Entwicklung von Windows-Apps.
ms.topic: article
ms.date: 03/19/2021
keywords: Windows 10, UWP, Toolkit SDK, WinUI, Windows-UI-Bibliothek
ms.openlocfilehash: 20f99f5e747d95b4ec0e806976393e209d3c2582
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730364"
---
# <a name="windows-ui-library-winui"></a>Windows UI Library (WinUI)

![WinUI-Logo](../images/logo-winui.png)

Die Windows-UI-Bibliothek (WinUI) ist ein natives Benutzererfahrungsframework (User Experience, UX) für Windows-Desktop- als auch UWP-Anwendungen.

Durch die Integration des [Fluent Design-Systems](https://www.microsoft.com/design/fluent/#/) in alle Erfahrungen, Steuerelemente und Stile bietet WinUI unter Verwendung der neuesten Benutzeroberflächenmuster (UI) einheitliche, intuitive und zugängliche Benutzerumgebungen.

Dank Unterstützung sowohl für Desktop- als auch für UWP-Apps können Sie mit WinUI eine App von Grund auf neu erstellen oder Ihre bestehenden MFC-, WinForms- oder WPF-Apps in vertrauten Sprachen wie C++, C#, Visual Basic und JavaScript (über [React Native für Windows](https://microsoft.github.io/react-native-windows/)) stufenweise migrieren.

> [!Important]
> Zwei Versionen stehen von WinUI zur Verfügung: **WinUI 2.x** und **WinUI 3**.

## <a name="windows-ui-2x-library"></a>Windows-UI 2.x-Bibliothek

WinUI 2.x kann in UWP-Anwendungen verwendet und über [XAML Islands](../desktop/modernize/xaml-islands.md) in neue oder bestehende Desktopanwendungen integriert werden.

> [!NOTE]
> WinUI 2.5 ist das neueste WinUI 2.x-Release. Im Artikel zum [WinUI 2.6-Meilenstein](https://github.com/microsoft/microsoft-ui-xaml/milestone/11) findest du eine Liste der für das nächste Release geplanten Entwicklungen.

Die WinUI 2.x-Bibliothek ist eng an das [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) gekoppelt und bietet offizielle native Windows-Benutzeroberflächen-Steuerelemente und andere Benutzeroberflächenelemente für UWP-Apps.

Durch Beibehalten von Abwärtskompatibilität mit früheren Versionen von Windows 10 wird ermöglicht, dass deine WinUI 2.x-Steuerelemente auch dann funktionieren, wenn Benutzer nicht über das aktuelle Betriebssystem verfügen.

![Unterstützung der WinUI 2.x-Plattform](../images/platforms-winui2.png)

**Installationsanweisungen finden Sie unter [Erste Schritte mit der Windows-UI-Bibliothek](winui2/getting-started.md).**

### <a name="related-links-for-winui-2x"></a>Verwandte Links für WinUI 2.x

- [Übersicht über die WinUI 2.x-Bibliothek](winui2/index.md)
- [API-Dokumentation](/windows/winui/api/)
- [Quellcode](https://aka.ms/winui)
- [XAML-Steuerelementekatalog-App](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-project-reunion-05"></a>Windows-UI-Bibliothek 3 (Project Reunion 0.5)

WinUI 3 ist die nächste WinUI-Version, eine Plattform für eine native Windows 10-Benutzeroberfläche, die komplett vom [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) entkoppelt ist.

> [!Important]
> WinUI 3 – Project Reunion 0.5 ist die erste stabile, unterstützte Version von WinUI 3. Mit dieser Version von WinUI 3 können Sie Produktions-Apps erstellen und im Microsoft Store veröffentlichen.
>
> Verwenden Sie das [GitHub-Repository für WinUI](https://github.com/microsoft/microsoft-ui-xaml), um Feedback zu geben und Vorschläge und Probleme zu protokollieren.

Durch die vollständige Entkoppelung von XAML, Komposition und Eingabe-APIs vom [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) umfasst der Umfang von WinUI 3 die vollständige Plattform für native Windows 10-Benutzeroberflächen.

WinUI 3 ist eine Komponente von [Project Reunion](../project-reunion/index.md), die einen einheitlichen Satz von APIs und Tools bietet, die von jeder Desktop-App auf einer breiten Palette von Zielversionen des Windows 10-Betriebssystems auf konsistente Weise verwendet werden können. Als Komponente von Project Reunion wird WinUI 3 als Teil des Project Reunion-Pakets ausgeliefert. Weitere Informationen finden Sie unter [Windows-UI-Bibliothek 3 – Project Reunion 0.5](winui3/index.md).

WinUI 3 ist der Pfad für die Weiterentwicklung aller Windows-Apps. Sie können es als Benutzeroberflächenschicht in Ihrer nativen UWP- oder Win32-App verwenden oder dazu nutzen, um Ihre Desktop-App schrittweise, nach und nach mithilfe von [XAML Islands](../desktop/modernize/xaml-islands.md) zu modernisieren.

Alle neuen XAML-Features werden letztendlich als Teil von WinUI bereitgestellt. Die bestehenden UWP-XAML-APIs, die als Teil des Betriebssystems bereitgestellt werden, erhalten keine neuen Featureupdates mehr. Sie erhalten jedoch weiterhin gemäß dem Supportlebenszyklus von Windows 10 Sicherheitsupdates und kritische Fehlerbehebungen.

Beachten Sie, dass die universelle Windows-Plattform mehr als nur das XAML-Framework enthält. Funktionen wie Anwendungs- und Sicherheitsmodelle, Medienpipeline, Xbox- und Windows 10-Shellintegrationen sowie Kompatibilität mit einer großen Vielzahl von Geräten werden weiterhin entwickelt und unterstützt.

![Support für die WinUI 3-Plattform](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>Verwandte Links für WinUI 3

- [Windows-UI-Bibliothek 3 – Project Reunion 0.5](winui3/index.md)
- [WinUI 3-Steuerelementekatalog – Beispiel-App](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3)

## <a name="winui-resources"></a>WinUI-Ressourcen

**GitHub**: WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Verwenden Sie das [WinUI-Repository](https://github.com/microsoft/microsoft-ui-xaml), um gewünschte Features anzufordern oder Programmfehler zu melden, mit dem WinUI-Team zu interagieren und die Pläne des Teams für WinUI 3 und darüber hinaus auf deren [Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) anzuzeigen.

**Website**: Auf der [WinUI-Website](https://aka.ms/winui) finden Sie Produktvergleiche, es werden die verschiedenen Vorteile von WinUI besprochen, und es werden Möglichkeiten bereitgestellt, mit dem Produkt und dem Produktteam in Verbindung zu bleiben.