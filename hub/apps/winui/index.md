---
title: Windows-UI-Bibliothek (WinUI)
description: WinUI-Bibliotheken für die Entwicklung von Windows-Apps.
ms.topic: article
ms.date: 05/11/2020
keywords: Windows 10, UWP, Toolkit SDK, WinUI, Windows-UI-Bibliothek
ms.openlocfilehash: 01eed4a82bfe14b70c86ade1b82c487e33e6f6f6
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775883"
---
# <a name="windows-ui-library-winui"></a>Windows-UI-Bibliothek (WinUI)

![Toolkits Favoritenbild](../images/logo-winui.png)

Die Windows UI-Bibliothek (WinUI) ist die Plattform für die moderne native Benutzeroberfläche (UI) von Windows-Apps für sowohl Win32 als auch UWP.

Durch die Integration des [Fluent Design-Systems](https://www.microsoft.com/design/fluent/#/) in alle Steuerelemente und Stile bietet WinUI unter Verwendung der neuesten Benutzeroberflächenmuster einheitliche, intuitive und zugängliche Benutzerumgebungen.

Dank Unterstützung sowohl für Win32- als auch für UWP-Apps kannst du mit WinUI eine App von Grund auf neu erstellen oder deine bestehenden MFC-, WinForms- oder WPF-Apps in deinem eigenen Tempo und mit vertrauten Sprachen wie C++, C#, Visual Basic und sogar JavaScript über [React Native für Windows](https://microsoft.github.io/react-native-windows/) migrieren.

Es gibt zwei WinUI-Versionen, die du kennen musst: **WinUI 2.x** und **WinUI 3.0**.

## <a name="windows-ui-2x-library"></a>Windows-UI 2.x-Bibliothek

WinUI 2.x kann unmittelbar in UWP-Anwendungen verwendet und über [XAML Islands](/windows/apps/desktop/modernize/xaml-islands) in deine bestehenden Desktopanwendungen integriert werden.

Die WinUI 2.x-Bibliothek ist eng an das UWP SDK gekoppelt und bietet offizielle native Windows-Benutzeroberflächen-Steuerelemente und andere Benutzeroberflächenelemente für UWP-Apps.

Durch Beibehalten von Abwärtskompatibilität mit früheren Versionen von Windows 10 wird ermöglicht, dass deine WinUI 2.x-Steuerelemente auch dann funktionieren, wenn Benutzer nicht über das aktuelle Betriebssystem verfügen.

![Unterstützung der WinUI 2.x-Plattform](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2.4 ist das neueste WinUI 2.x-Release. Im Artikel zum [WinUI 2.5-Meilenstein](https://github.com/microsoft/microsoft-ui-xaml/milestone/10) findest du eine Liste der für das nächste Release geplanten Entwicklungen.

Installationsanweisungen findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](winui2/getting-started.md).

### <a name="related-links-for-winui-2x"></a>Verwandte Links für WinUI 2.x

- [Übersicht über die WinUI 2.x-Bibliothek](winui2/index.md)
- [API-Dokumentation](https://docs.microsoft.com/uwp/api/overview/winui/)
- [Quellcode](https://aka.ms/winui)
- [XAML-Steuerelementekatalog-App](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Windows UI 3.0-Bibliothek (Vorschau 1)

WinUI 3 ist die nächste WinUI-Version, eine Plattform für eine native Windows 10-Benutzeroberfläche, die komplett vom UWP SDK entkoppelt ist.

Durch die vollständige Entkoppelung von XAML, Komposition und Eingabe-APIs vom UWP SDK kann die Windows UI 3.0-Bibliothek den Umfang von WinUI erheblich erweitern, um die Plattform für die komplette native Windows 10-Benutzeroberfläche einzubeziehen.

WinUI fungiert als Pfad für die Weiterentwicklung aller Windows-Apps. Du kannst es als Benutzeroberflächenschicht in deiner nativen UWP- oder Win32-App verwenden oder dazu nutzen, um deine Desktop-App nach und nach mithilfe von [XAML Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands) zu modernisieren.

> [!NOTE]
> WinUI 3.0 Vorschau 1 ist ein Vorabversionsbuild von WinUI 3.0. Wir freuen uns über dein Feedback im [GitHub-Repository von WinUI](https://github.com/microsoft/microsoft-ui-xaml).

Alle neuen XAML-Features werden als Teil von WinUI bereitgestellt. Die bestehenden UWP-XAML-APIs, die als Teil des Betriebssystems bereitgestellt werden, erhalten keine neuen Updates von Features mehr. Sie erhalten jedoch weiterhin gemäß dem Supportlebenszyklus von Windows 10 Sicherheitsupdates und kritische Fehlerbehebungen.

Die Universelle Windows-Plattform enthält mehr als bloß das XAML-Framework (wie z. B. Anwendungs- und Sicherheitsmodelle, Medienpipeline, Xbox- und Windows 10-Shellintegrationen, umfangreiche Geräteunterstützung) und erhält weiterhin Support.

![Support für die WinUI 3.0-Plattform](../images/platforms-winui3.png)

> [!Important]
> WinUI 3.0 Vorschau 1 ist für eine frühe Evaluierung und zum Sammeln von Feedback von der Entwicklercommunity vorgesehen. Diese Version sollte **NICHT** für Produktions-Apps verwendet werden.

### <a name="related-links-for-winui-30"></a>Verwandte Links für WinUI 3.0

- [WinUI 3.0 Vorschau 1 (Mai 2020)](winui3/index.md)
- [XAML-Steuerelementkatalog-App (WinUI 3.0 Vorschau 1)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI-Ressourcen

**GitHub**: WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Im [WinUI-Repository](https://github.com/microsoft/microsoft-ui-xaml) kannst du gewünschte Features oder Programmfehler melden, mit dem WinUI-Team interagieren und die Pläne des Teams für WinUI 3 und darüber hinaus auf seiner [Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) einsehen.

**Website**: Auf der [WinUI-Website](https://aka.ms/winui) findest du hilfreiche Informationen zu Produktvergleichen, den Vorteilen von WinUI und Möglichkeiten, mit dem Produkt und dem Produktteam in Verbindung zu bleiben.
