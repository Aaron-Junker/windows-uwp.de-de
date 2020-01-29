---
description: Diese Anleitung hilft Ihnen, Fluent-basierte UWP-UIs direkt in einer WPF- oder Windows Forms-Anwendung zu erstellen.
title: UWP-Steuerelemente in Desktopanwendungen
ms.date: 01/10/2010
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 95fbfc9aa988330fb21713651687690fa769b99f
ms.sourcegitcommit: 85fd390b1e602707bd9342cb4b84b97ae0d8b831
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76520405"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hosten von UWP XAML-Steuerelementen in Desktop-Apps (XAML Islands)

Ab Windows 10, Version 1903, können Sie mithilfe eines Features namens *XAML Islands* UWP-Steuerelemente auch in anderen als UWP-Desktop-Anwendungen hosten. Mit diesem Feature können Sie Aussehen, Handhabung und Funktionalität Ihrer vorhandenen WPF-, Windows Forms- und C++-Win32-Anwendungen mit den aktuellen Features der Windows 10-Benutzeroberfläche verbessern, die nur in Form von UWP-Steuerelementen verfügbar sind. Dies bedeutet, dass Sie UWP-Features, wie etwa [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions), und Steuerelemente, die das [Fluent Design-System](/windows/uwp/design/fluent-design-system/index) unterstützen, in Ihren vorhandenen WPF-, Windows Forms- und C++-Win32-Anwendungen nutzen können.

Sie können beliebige UWP-Steuerelemente hosten, die von [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) abgeleitet sind, darunter:

* Alle vom Originalhersteller stammenden UWP-Steuerelemente, die im Windows SDK oder der WinUI-Bibliothek bereitstehen.
* Alle benutzerdefinierten UWP-Steuerelemente (beispielsweise ein Benutzersteuerelement, das aus mehreren UWP-Steuerelementen besteht, die zusammenarbeiten). Sie müssen über den Quellcode für das benutzerdefinierte Steuerelement verfügen, damit Sie es mit Ihrer Anwendung kompilieren können.

Grundsätzlich werden XAML Islands mithilfe der *UWP XAML-Hosting-API* erstellt. Diese API besteht aus mehreren Windows-Runtime Klassen und COM-Schnittstellen, die im SDK für Windows 10, Version 1903, eingeführt wurden. Wir stellen darüber hinaus eine Reihe von XAML Island-.NET-Steuerelementen im [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) zur Verfügung, die die UWP XAML-Hosting-API intern verwenden und für WPF- und Windows Forms-Apps ein komfortableres Entwicklungserlebnis bieten.

Die Art, wie Sie XAML Islands verwenden, hängt von Ihrem Anwendungstyp und den Typen der UWP-Steuerelemente ab, die Sie hosten möchten.

> [!NOTE]
> Wenn Sie Feedback zu XAML Islands haben, erstellen Sie ein neues Thema im [Microsoft.Toolkit.Win32-Repository](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues), und geben Sie dort Ihre Kommentare ab. Wenn Sie Ihr Feedback lieber privat geben möchten, können Sie es an XamlIslandsFeedback@microsoft.com senden. Ihre Erkenntnisse und Szenarien sind äußerst wichtig für uns.

## <a name="wpf-and-windows-forms-applications"></a>WPF- und Windows Forms-Anwendungen

Wir empfehlen, für WPF- und Windows Forms-Anwendungen die XAML Island-.NET-Steuerelemente zu verwenden, die im Windows Community Toolkit verfügbar sind. Diese Steuerelemente bieten ein Objektmodell, das die Eigenschaften, Methoden und Ereignisse der entsprechenden UWP-Steuerelemente nachahmt (oder Zugriff auf sie bietet). Darüber hinaus verarbeiten sie Verhalten wie Tastaturnavigation und Layoutänderungen.

Es gibt zwei Sätze von XAML Island-Steuerelementen für WPF- und Windows Forms-Anwendungen: *umschließende Steuerelemente* und *Hoststeuerelemente*. 

### <a name="wrapped-controls"></a>Umschließende Steuerelemente

WPF- und Windows Forms-Anwendungen können eine Reihe von XAML Island-Steuerelementen verwenden, die die Schnittstelle und die Funktionalität eines bestimmten UWP-Steuerelements umschließen. Sie können diese Steuerelemente direkt der Entwurfsoberfläche Ihres WPF- oder Windows Forms-Projekts hinzufügen und sie dann wie jedes andere WPF- oder Windows Forms-Steuerelement im Designer verwenden.

Aktuell stehen die folgenden umschließenden UWP-Steuerelemente im Windows Community Toolkit zur Verfügung. 

| Control | Unterstützte Mindestversion des Betriebssystems | Beschreibung |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, Version 1903 | Stellen Sie eine Oberfläche und zugehörige Symbolleisten für die Windows Ink-basierte Benutzerinteraktion in Ihrer Windows Forms- oder WPF-Desktop Anwendung bereit. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, Version 1903 | Bettet eine Ansicht ein, die Medieninhalte wie etwa Videoclips in Ihrer Windows Forms- oder WPF-Desktop-Anwendung streamt und rendert. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, Version 1903 | Ermöglicht Ihnen das Anzeigen einer symbolischen oder fotorealistischen Karte in Ihrer Windows Forms- oder WPF-Desktop-Anwendung. |

Eine exemplarische Vorgehensweise, die die Verwendung der umschließenden UWP-Steuerelemente veranschaulicht, finden Sie unter [Hosten eines UWP-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Hoststeuerelemente

Für Szenarien, die über die mit den verfügbaren umschließenden Steuerelementen gegebenen Möglichkeiten hinausgehen, kann in WPF- und Windows Forms-Anwendungen auch das [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelement eingesetzt werden, das im Windows Community Toolkit zur Verfügung steht.

| Control | Unterstützte Mindestversion des Betriebssystems | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, Version 1903 | Kann ein beliebiges UWP-Steuerelement hosten, das von [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) abgeleitet ist, einschließlich der vom Windows SDK bereitgestellten UWP-Originalsteuerelemente sowie benutzerdefinierter Steuerelemente. |

Exemplarische Vorgehensweisen, in denen die Verwendung des **WindowsXamlHost**-Steuerelements veranschaulicht wird, finden Sie unter [Hosten eines UWP-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md) und [Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App mithilfe von XAML Islands](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> Die Verwendung des **WindowsXamlHost**-Steuerelements zum Hosten benutzerdefinierter UWP-Steuerelemente wird nur in WPF- und Windows Forms-Apps für die Zielplattform .NET Core 3 unterstützt. Das Hosten von UWP-Originalsteuerelementen aus dem Windows SDK wird in Apps für die Zielplattformen .NET Framework und .NET Core 3 unterstützt.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Konfigurieren des Projekts zum Verwenden der XAML Island-.NET-Steuerelemente

Für die XAML Island-.NET-Steuerelemente ist Windows 10, Version 1903, oder eine höhere Version erforderlich. Installieren Sie eins der unten aufgeführten NuGet-Pakete, um diese Steuerelemente zu verwenden. Diese Pakete bieten alles, was Sie benötigen, um die umschließenden XAML Island-Steuerelemente und die XAML Island-Hoststeuerelemente zu verwenden, und sie enthalten weitere verwandte NuGet-Pakete, die ebenfalls benötigt werden.

| Steuerelementtyp | NuGet-Paket  | Verwandte Artikel |
|-----------------|----------------|---------------------|
| [Umschließende Steuerelemente](#wrapped-controls) | Version 6.0.0 oder höher dieser Pakete: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hosten eines UWP-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md)  |
| [Hoststeuerelement](#host-controls) | Version 6.0.0 oder höher dieser Pakete: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hosten eines UWP-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md)<br/>[Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App](host-custom-control-with-xaml-islands.md)  |

Bitte beachten Sie die folgenden Details:

* Die Pakete mit den umschließenden Steuerelementen enthalten außerdem die Pakete mit den Hoststeuerelementen. Sie können die Pakete mit den umschließenden Steuerelementen installieren, wenn Sie beide Steuerelementsätze verwenden möchten.

* Wenn Sie ein benutzerdefiniertes UWP-Steuerelement hosten, muss Ihr WPF- oder Windows Forms-Projekt für .NET Core 3 ausgelegt sein. Das Hosten von benutzerdefinierten UWP-Steuerelementen wird in Apps für die Zielplattform .NET Framework nicht unterstützt. Außerdem müssen Sie einige weitere Schritte ausführen, um auf das benutzerdefinierte Steuerelement zu verweisen. Weitere Informationen finden Sie unter [Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App mithilfe von XAML Islands](host-custom-control-with-xaml-islands.md).

* In früheren Versionen dieser Anweisungen wurden Sie aufgefordert, dem Anwendungsmanifest in Ihrem WPF- oder Windows Forms-Projekt das `maxversiontested`-Element hinzuzufügen. Sofern Sie die neuesten Versionen der oben aufgeführten NuGet-Pakete verwenden, brauchen Sie Ihrem Manifest dieses Element nicht mehr hinzuzufügen.

### <a name="architecture-of-xaml-island-net-controls"></a>Architektur der XAML Island-.NET-Steuerelemente

Hier geben wir einen kurzen Überblick zur architektonischen Gliederung der XAML Island-Steuerelemente auf dem Fundament der UWP XAML-Hosting-API.

![Hoststeuerelement-Architektur](images/xaml-islands/host-controls.png)

Die am Diagrammende aufgeführten APIs sind im Lieferumfang des Windows SDK enthalten. Die umschließenden Steuerelemente und die Hoststeuerelemente stehen im Windows Community Toolkit in Form von NuGet-Paketen zur Verfügung.

### <a name="web-view-controls"></a>Webansicht-Steuerelemente

Das Windows Community Toolkit stellt außerdem die folgenden .NET-Steuerelemente für das Hosten von Webinhalten in WPF- und Windows Forms-Anwendungen zur Verfügung. Diese Steuerelemente werden häufig in ähnlichen Szenarien zur Modernisierung von Desktop-Apps verwendet wie die XAML Island-Steuerelemente, und sie werden im gleichen [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)-Repository wie die XAML Island-Steuerelemente verwaltet.

| Control | Unterstützte Mindestversion des Betriebssystems | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, Version 1803 | Verwendet die Microsoft Edge-Renderingengine zum Anzeigen von Webinhalten. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Bietet eine Version von **WebView**, die mit mehr Betriebssystemversionen kompatibel ist. Dieses Steuerelement verwendet die Microsoft Edge-Renderingengine, um Webinhalte unter Windows 10, Version 1803 und höher, und die Internet Explorer-Renderingengine, um Webinhalte in früheren Windows 10-Versionen sowie Windows 8.x und Windows 7 anzuzeigen. |

Installieren Sie eins dieser NuGet-Pakete, um diese Steuerelemente zu verwenden:

* WPF: [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32-Anwendungen

Die XAML Island-.NET-Steuerelemente werden in C++ Win32-Anwendungen nicht unterstützt. Diese Anwendungen müssen stattdessen die *UWP XAML-Hosting-API* verwenden, die vom Windows 10 SDK bereitgestellt wird (Version 1903 und höher).

Die UWP XAML-Hosting-API besteht aus verschiedenen Windows Runtime-Klassen und COM-Schnittstellen, die von Ihrer C++ Win32-Anwendung zum Hosten beliebiger UWP-Steuerelemente verwendet werden können, die von [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) abgeleitet sind. UWP-Steuerelemente lassen sich in jedem beliebigen Element der Benutzeroberfläche hosten, dem ein Fensterhandle (HWND) zugeordnet ist. Weitere Informationen zu dieser API, einschließlich der Voraussetzungen, finden Sie unter [Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App](using-the-xaml-hosting-api.md).

> [!NOTE]
> Die umschließenden Steuerelemente und Hoststeuerelemente im Windows Community Toolkit verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, einschließlich Tastaturnavigation und Layoutänderungen, um das Sie sich andernfalls, bei direkter Verwendung der UWP XAML-Hosting-API, selbst kümmern müssten. Für WPF- und Windows Forms-Anwendungen empfehlen wir dringend, diese Steuerelemente zu nutzen, statt die UWP XAML-Hosting-API direkt zu verwenden, da sie Ihnen durch Abstraktion viele der Implementierungsdetails ersparen, die mit der Verwendung der API einhergehen.

## <a name="feature-roadmap"></a>Featureroadmap

Hier finden Sie den aktuellen Status der mit XAML Islands zusammenhängenden Features nach dem Stand von Windows 10, Version 1903, und der Version 6.0 des Windows Community Toolkit:

* **C++-Win32-Apps:** Die UWP XAML-Hosting-API gilt auf dem Stand von Windows 10, Version 1903, als Version 1.0.
* **Verwaltete Apps für die Zielplattform .NET Framework 4.6.2 und höher:** Die XAML Island-Steuerelemente, die in den [NuGet-Paketen der Version 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls) verfügbar sind, werden für Apps mit der Zielplattform .NET Framework 4.6.2 und höher als Version 1.0 angesehen.
* **Verwaltete Apps für die Zielplattform .NET Core 3.0 und höher:** Die in den [NuGet-Paketen der Version 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls) verfügbaren Steuerelemente befinden sich für Apps für die Zielplattform .NET Core 3.0 und höher noch in der Developer Preview. Die Version 1.0 dieser Steuerelemente für .NET Core 3.0 und höher ist für eine spätere Version geplant.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Hintergrundinformationen und Tutorials zur Verwendung von XAML Islands finden Sie in den folgenden Artikeln und Ressourcen:

* [Tutorial Modernisieren einer WPF-App](modernize-wpf-tutorial.md): Dieses Tutorial enthält schrittweise Anleitungen zum Verwenden der umschließenden Steuerelemente und Hoststeuerelemente im Windows Community Toolkit zum Hinzufügen von UWP-Steuerelementen zu einer vorhandenen WPF-Branchenanwendung. Dieses Tutorial enthält den vollständigen Code für die WPF-Anwendung sowie ausführliche Anweisungen zu den einzelnen Schritten in diesem Prozess.
* [XAML Islands v1 – Updates and Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): In diesem Blogbeitrag werden viele häufig gestellte Fragen zu XAML Islands erläutert und eine ausführliche Entwicklungsroadmap zur Verfügung gestellt.
