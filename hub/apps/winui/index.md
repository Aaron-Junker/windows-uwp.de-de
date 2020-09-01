---
title: Windows-UI-Bibliothek (WinUI)
description: WinUI-Bibliotheken für die Entwicklung von Windows-Apps.
ms.topic: article
ms.date: 07/15/2020
keywords: Windows 10, UWP, Toolkit SDK, WinUI, Windows-UI-Bibliothek
ms.openlocfilehash: eb87744ed5d3eb5882b4ebae75b8dcf295d89f10
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166754"
---
# <a name="windows-ui-library-winui"></a>Windows UI Library (WinUI)

![WinUI-Logo](../images/logo-winui.png)

Die Windows-UI-Bibliothek (WinUI) ist ein natives Benutzererfahrungs-Framework (User Experience, UX) für Windows-Desktop- als auch UWP-Anwendungen.

Durch die Integration des [Fluent Design-Systems](https://www.microsoft.com/design/fluent/#/) in alle Erfahrungen, Steuerelemente und Stile bietet WinUI unter Verwendung der neuesten Benutzeroberflächenmuster (UI) einheitliche, intuitive und zugängliche Benutzerumgebungen.

Dank Unterstützung sowohl für Desktop- als auch für UWP-Apps können Sie mit WinUI eine App von Grund auf neu erstellen oder Ihre bestehenden MFC-, WinForms- oder WPF-Apps in vertrauten Sprachen wie C++, C#, Visual Basic und JavaScript (über [React Native für Windows](https://microsoft.github.io/react-native-windows/)) stufenweise migrieren.

> [!Important]
> Zwei Versionen stehen von WinUI zur Verfügung: **WinUI 2.x** und **WinUI 3**.

## <a name="windows-ui-2x-library"></a>Windows-UI 2.x-Bibliothek

WinUI 2.x kann in UWP-Anwendungen verwendet und über [XAML Islands](../desktop/modernize/xaml-islands.md) in neue oder bestehende Desktopanwendungen integriert werden.

> [!NOTE]
> WinUI 2.4 ist das neueste WinUI 2.x-Release. Im Artikel zum [WinUI 2.5-Meilenstein](https://github.com/microsoft/microsoft-ui-xaml/milestone/10) findest du eine Liste der für das nächste Release geplanten Entwicklungen.

Die WinUI 2.x-Bibliothek ist eng an das [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) gekoppelt und bietet offizielle native Windows-Benutzeroberflächen-Steuerelemente und andere Benutzeroberflächenelemente für UWP-Apps.

Durch Beibehalten von Abwärtskompatibilität mit früheren Versionen von Windows 10 wird ermöglicht, dass deine WinUI 2.x-Steuerelemente auch dann funktionieren, wenn Benutzer nicht über das aktuelle Betriebssystem verfügen.

![Unterstützung der WinUI 2.x-Plattform](../images/platforms-winui2.png)

**Installationsanweisungen finden Sie unter [Erste Schritte mit der Windows-UI-Bibliothek](winui2/getting-started.md).**

### <a name="related-links-for-winui-2x"></a>Verwandte Links für WinUI 2.x

- [Übersicht über die WinUI 2.x-Bibliothek](winui2/index.md)
- [API-Dokumentation](/uwp/api/overview/winui/)
- [Quellcode](https://aka.ms/winui)
- [XAML-Steuerelementekatalog-App](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-preview-2"></a>Windows UI 3-Bibliothek (Vorschau 2)

WinUI 3 ist die nächste WinUI-Version, eine Plattform für eine native Windows 10-Benutzeroberfläche, die komplett vom [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) entkoppelt ist.

> [!Important]
> Die WinUI 3-Vorschauversion ist für eine frühe Evaluierung und zum Sammeln von Feedback aus der Entwicklercommunity vorgesehen. Diese Version sollte **NICHT** für Produktions-Apps verwendet werden.
>
> Wir werden weiterhin die Vorschauversionen von WinUI 3 in ganz 2020 und bis ins frühe 2021 veröffentlichen. Danach wird das erste offizielle Release verfügbar gemacht.
>
> Verwenden Sie das [GitHub-Repository für WinUI](https://github.com/microsoft/microsoft-ui-xaml), um Feedback zu geben und Vorschläge und Probleme zu protokollieren.

Durch die vollständige Entkoppelung von XAML, Komposition und Eingabe-APIs vom [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) umfasst der Umfang von WinUI 3 die vollständige Plattform für native Windows 10-Benutzeroberflächen.

WinUI ist der Pfad für die Weiterentwicklung aller Windows-Apps. Sie können es als Benutzeroberflächenschicht in Ihrer nativen UWP- oder Win32-App verwenden oder dazu nutzen, um Ihre Desktop-App schrittweise, nach und nach mithilfe von [XAML Islands](../desktop/modernize/xaml-islands.md) zu modernisieren.

Alle neuen XAML-Features werden letztendlich als Teil von WinUI bereitgestellt. Die bestehenden UWP-XAML-APIs, die als Teil des Betriebssystems bereitgestellt werden, erhalten keine neuen Featureupdates mehr. Sie erhalten jedoch weiterhin gemäß dem Supportlebenszyklus von Windows 10 Sicherheitsupdates und kritische Fehlerbehebungen.

Beachten Sie, dass die universelle Windows-Plattform mehr als nur das XAML-Framework enthält. Funktionen wie Anwendungs- und Sicherheitsmodelle, Medienpipeline, Xbox- und Windows 10-Shellintegrationen sowie Kompatibilität mit einer großen Vielzahl von Geräten werden weiterhin entwickelt und unterstützt.

![Support für die WinUI 3-Plattform](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>Verwandte Links für WinUI 3

- [Windows-UI-Bibliothek 3 Vorschau 2 (Juli 2020)](winui3/index.md)
- [XAML-Steuerelementkatalog-App (WinUI 3 Vorschau 2)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI-Ressourcen

**GitHub**: WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Verwenden Sie das [WinUI-Repository](https://github.com/microsoft/microsoft-ui-xaml), um gewünschte Features anzufordern oder Programmfehler zu melden, mit dem WinUI-Team zu interagieren und die Pläne des Teams für WinUI 3 und darüber hinaus auf deren [Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) anzuzeigen.

**Website**: Auf der [WinUI-Website](https://aka.ms/winui) finden Sie Produktvergleiche, es werden die verschiedenen Vorteile von WinUI besprochen, und die Site hilft Ihnen, mit dem Produkt und dem Produktteam in Verbindung zu bleiben.