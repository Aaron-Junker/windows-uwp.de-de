---
title: Vergleich von WinUI 3 und WinUI 2
description: Kurze Gegenüberstellung von WinUI 3 und WinUI 2.
ms.topic: article
ms.date: 03/19/2021
keywords: Windows 10, UWP, Toolkit SDK, WinUI, Windows-UI-Bibliothek
ms.openlocfilehash: 8c415b3c08e0f7cd215cd99f2c54fc3cb760d3e0
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730679"
---
# <a name="comparison-of-winui-3-and-winui-2"></a>Vergleich von WinUI 3 und WinUI 2

:::image type="content" source="../images/logo-winui2-vs-winui3.png" alt-text="Logos von Win UI 2 und Win UI 3":::

Derzeit gibt es zwei verschiedene Generationen der Windows-UI-Bibliothek (WinUI): WinUI 2 und WinUI 3. Beide bieten zwar Benutzeroberflächen und Funktionen, die auf den neuesten [Fluent Design System](https://www.microsoft.com/design/fluent)-Prinzipien und -Prozessen basieren, haben aber verschiedene Entwicklungsziele und -bereiche mit unterschiedlichen Entwicklungspfaden und Veröffentlichungszeitplänen.

Sowohl [WinUI 2](winui2/index.md) als auch [WinUI 3](winui3/index.md) unterstützen die Entwicklung von produktionsbereiten Apps unter Windows 10, und in beiden können Sie Ihre Apps unter Verwendung von C# oder C++/Win32 erstellen.

Beide werden mit Updates aktiv weiterentwickelt, die in regelmäßigen Abständen veröffentlicht werden.

## <a name="the-major-differences"></a>Die Hauptunterschiede

| WinUI 3                                                                                                                                                                                                                                                                                                        | WinUI 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| UX-Stack und Steuerungsbibliothek sind vollständig vom Betriebssystem und dem [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) entkoppelt, einschließlich Kernframework, Kompositions- und Eingabeebene des UX-Stacks.                                                                        | UX-Stack und die Steuerelementbibliothek sind eng an das Betriebssystem und das [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) gekoppelt.                                                                                                                                                                                                                                                                                                                                                          |
| WinUI 3 kann verwendet werden, um produktionsbereite Windows-**Desktop/Win32**-Apps zu erstellen. | WinUI 2 kann nicht zum Erstellen von Windows-**Desktop-/Win32**-Apps verwendet werden. |
| WinUI 3 wird als Komponente des [Project Reunion](../project-reunion/index.md)-Frameworkpakets mit Visual Studio-Projektvorlagen in der Project Reunion Visual Studio-Erweiterung (VSIX) ausgeliefert. | Ein Teil von WinUI 2 wird im Betriebssystem selbst (die Windows.UI.*-Familie der UWP-WinRT-APIs) und ein Teil wird als Bibliothek („Windows-UI-Bibliothek 2“) mit zusätzlichen Steuerelementen, Elementen und den neuesten Formatvorlagen, zusätzlich zu dem, was bereits im Betriebssystem selbst enthalten ist, ausgeliefert. Mit WinUI 2 werden diese Features in einem herunterladbaren NuGet-Paket ausgeliefert. Andere bedeutende Teile des UI-Stacks sind jedoch noch in das Betriebssystem integriert, z. B. XAML-Kernframework, Eingabe- und Kompositionsebene. |
| WinUI 3 unterstützt C#/.NET 5 für Desktop-Apps. | WinUI 2 unterstützt nur native C#/.NET-Apps. |
| WinUI 3-Unterstützung für produktionsbereite UWP-Apps befindet sich derzeit in der Vorschau. Weitere Informationen finden Sie unter [WinUI 3 – Project Reunion 0.5 Preview](winui3/release-notes/winui3-project-reunion-0.5-preview.md).                                                                                                                                | WinUI 2 kann in UWP-Apps für die Produktion integriert werden, indem ein NuGet-Paket in einem neuen oder vorhandenen UWP-Projekt installiert wird. Auf WinUI-Steuerelemente und -Formatvorlagen kann dann direkt in neuen Apps oder durch Aktualisieren von „windows.ui“ verwiesen werden. Namespaceverweise auf „microsoft.ui“ in vorhandenen Apps.                                                                                                                                                                                    |
| WinUI 3 unterstützt das Chromium-basierte [WebView2](/microsoft-edge/webview2/)-Steuerelement. |  WinUI 2 unterstützt das [WebView](/windows/uwp/design/controls-and-patterns/web-view)-Steuerelement. |
| WinUI 3 ist abwärtskompatibel mit Windows 10 October 2018 Update (Version 1809, Betriebssystembuild 17763). | WinUI 2 ist abwärtskompatibel mit Windows 10 Creators Update (Version 1703, Betriebssystembuild 15063). |

## <a name="see-also"></a>Weitere Informationen

- [Erstellen von Windows-Desktop-Apps mit Project Reunion 0.5](../project-reunion/index.md)

- [Windows-UI-Bibliothek (WinUI)](index.md)

- [Windows-UI-Bibliothek 2.x](winui2/index.md)

- [Windows-UI-Bibliothek 3 – Project Reunion 0.5 (März 2021)](winui3/index.md)
