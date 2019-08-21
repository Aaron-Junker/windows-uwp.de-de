---
description: Diese Anleitung hilft Ihnen, Fluent-basierte UWP-UIs direkt in einer WPF- oder Windows Forms-Anwendung zu erstellen.
title: UWP-Steuerelemente in Desktop-Apps
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bd49417d110759dc9fec4ff4c9003e842bf1d7bb
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643348"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)

Ab Windows 10, Version 1903, können Sie UWP-Steuerelemente in nicht-UWP-Desktop Anwendungen mit einer Funktion namens " *XAML-Inseln*" hosten. Diese Funktion ermöglicht es Ihnen, das Aussehen, das Gefühl und die Funktionalität Ihrer vorhandenen WPF-, Windows Forms- C++ und Win32-Anwendungen mit den neuesten Windows 10-Benutzeroberflächen Features zu verbessern, die nur über UWP-Steuerelemente zur Verfügung stehen. Dies bedeutet, dass Sie UWP-Funktionen wie z. b. [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) und Steuerelemente verwenden können, die das [fließende Entwurfs System](/windows/uwp/design/fluent-design-system/index) in Ihren vorhandenen C++ WPF-, Windows Forms-und Win32-Anwendungen unterstützen.

Sie können ein beliebiges UWP-Steuerelement hosten, das von [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)abgeleitet wird, einschließlich:

* Alle UWP-Steuerelemente, die von der Windows SDK-oder WinUI-Bibliothek bereitgestellt werden.
* Jedes benutzerdefinierte UWP-Steuerelement (z. b. ein Benutzer Steuerelement, das aus mehreren UWP-Steuerelementen besteht, die zusammenarbeiten). Sie müssen über den Quellcode für das benutzerdefinierte Steuerelement verfügen, damit Sie es mit Ihrer Anwendung kompilieren können.

Im Grunde werden XAML-Inseln mit der *UWP-XAML-Hosting-API*erstellt. Diese API besteht aus mehreren Windows-Runtime Klassen und COM-Schnittstellen, die im SDK für Windows 10, Version 1903, eingeführt wurden. Wir stellen auch eine Reihe von XAML-Steuerelementen für die .net-Insel im [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) bereit, die intern die UWP-XAML-Hosting-API verwenden und eine komfortablere Entwicklungsumgebung für WPF-und Windows Forms-Apps bieten.

Wie Sie XAML-Inseln verwenden, hängt von Ihrem Anwendungstyp und den Typen von UWP-Steuerelementen ab, die Sie hosten möchten.

> [!NOTE]
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem im Repository " [Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) ", und lassen Sie Ihre Kommentare dort ablegen. Wenn Sie Ihr Feedback lieber privat einreichen möchten, können Sie es an XamlIslandsFeedback@microsoft.comsenden. Ihre Einblicke und Szenarios sind für uns äußerst wichtig.

## <a name="wpf-and-windows-forms-applications"></a>WPF-und Windows Forms Anwendungen

Es wird empfohlen, dass WPF-und Windows Forms Anwendungen die XAML-Steuerelemente für die .net-Insel verwenden, die im Windows Community Toolkit verfügbar sind. Diese Steuerelemente stellen ein Objektmodell bereit, das die Eigenschaften, Methoden und Ereignisse der entsprechenden UWP-Steuerelemente imitiert (oder Zugriff darauf bietet). Außerdem behandeln Sie Verhalten wie Tastaturnavigation und Layoutänderungen.

Es gibt zwei Sätze von XAML-Insel Steuerelementen für WPF-und Windows Forms-Anwendungen: umschließende Steuer *Elemente* und *Host Steuerelemente*. Ab Windows 10, Version 1903, sind diese Steuerelemente [als Entwicklervorschau verfügbar](#feature-roadmap).

### <a name="wrapped-controls"></a>Umschließt Steuerelemente

WPF-und Windows Forms Anwendungen können eine Auswahl von XAML-Insel Steuerelementen verwenden, die die Schnittstelle und Funktionalität eines bestimmten UWP-Steuer Elements einschließen. Sie können diese Steuerelemente direkt zur Entwurfs Oberfläche Ihres WPF-oder Windows Forms Projekts hinzufügen und diese dann wie jedes andere WPF-oder Windows Forms-Steuerelement im Designer verwenden.

Die folgenden umschließbaren UWP-Steuerelemente sind zurzeit im Windows Community Toolkit verfügbar. 

| Steuerelement | Mindestens unterstütztes Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, Version 1903 | Stellen Sie eine Oberfläche und zugehörige Symbolleisten für die Windows Ink-basierte Benutzerinteraktion in Ihrer Windows Forms-oder WPF-Desktop Anwendung bereit. |
| [Mediaplayerelement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, Version 1903 | Bettet eine Ansicht ein, die Medieninhalte wie z. b. Videos in Ihrer Windows Forms-oder WPF-Desktop Anwendung streamt und rendert |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, Version 1903 | Ermöglicht es Ihnen, eine symbolische oder eine Fotoansicht in der Windows Forms-oder WPF-Desktop Anwendung anzuzeigen. |

Eine exemplarische Vorgehensweise, die die Verwendung der umschließenen UWP-Steuerelemente veranschaulicht, finden Sie unter [Hosten eines UWP-Standard Steuer Elements in einer WPF-App](host-standard-control-with-xaml-islands.md)

### <a name="host-controls"></a>Host Steuerelemente

Für Szenarien, die über diejenigen hinausgehen, die von den verfügbaren umschlenenen Steuerelementen abgedeckt werden, können WPF-und Windows Forms Anwendungen auch das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement verwenden, das im Windows Community Toolkit

| Steuerelement | Mindestens unterstütztes Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, Version 1903 | Kann ein beliebiges UWP-Steuerelement hosten, das von [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)abgeleitet wird, einschließlich der von der Windows SDK bereitgestellten UWP-Steuerelemente von UWP und benutzerdefinierter Steuerelemente. |

Exemplarische Vorgehensweisen, die veranschaulichen, wie das **Windows xamlhost** -Steuerelement verwendet wird, finden Sie unter [Hosten eines UWP-Standard Steuer Elements in einer WPF-App](host-standard-control-with-xaml-islands.md) und [Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App mit XAML-Inseln](host-custom-control-with-xaml-islands.md)

> [!NOTE]
> Das Verwenden des **windowsxamlhost** -Steuer Elements zum Hosten von benutzerdefinierten UWP-Steuerelementen wird nur in WPF-und Windows Forms-Apps unterstützt, die auf .net Core Das Hosting von UWP-Steuerelementen, die vom Windows SDK bereitgestellt werden, wird in Apps unterstützt, die auf die .NET Framework oder .net Core 3 abzielen.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Konfigurieren des Projekts für die Verwendung der XAML-Insel-. NET-Steuerelemente

Die .NET-Steuerelemente der XAML-Insel erfordern Windows 10, Version 1903 oder eine höhere Version. Wenn Sie diese Steuerelemente verwenden möchten, installieren Sie eines der unten aufgeführten nuget-Pakete. Diese Pakete enthalten alles, was Sie benötigen, um die von der XAML-Insel umschließenen Steuerelemente und Host Steuerelemente zu verwenden, und Sie enthalten andere zugehörige nuget-Pakete.

| Typ des Steuer Elements | Nuget-Paket  | Verwandte Artikel |
|-----------------|----------------|---------------------|
| [Umschließt Steuerelemente](#wrapped-controls) | Version 6.0.0-preview7 oder höher dieser Pakete: <ul><li>WPF [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hosten eines UWP-Standard Steuer Elements in einer WPF-App](host-standard-control-with-xaml-islands.md)  |
| [Host Steuerelement](#host-controls) | Version 6.0.0-preview7 oder höher dieser Pakete: <ul><li>WPF [Microsoft. Toolkit. WPF. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hosten eines UWP-Standard Steuer Elements in einer WPF-App](host-standard-control-with-xaml-islands.md)<br/>[Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App](host-custom-control-with-xaml-islands.md)  |

Beachten Sie die folgenden Details:

* Die Host Steuerelement Pakete sind auch in den umschließenden Steuerelement Paketen enthalten. Sie können die umschließenden Steuerelement Pakete installieren, wenn Sie beide Sätze von Steuerelementen verwenden möchten.

* Wenn Sie ein benutzerdefiniertes UWP-Steuerelement verwenden, muss das WPF-oder Windows Forms Projekt auf .net Core 3 ausgerichtet sein. Das Hosting von benutzerdefinierten UWP-Steuerelementen wird in apps nicht unterstützt, die auf .NET Framework abzielen. Außerdem müssen Sie einige zusätzliche Schritte ausführen, um auf das benutzerdefinierte Steuerelement zu verweisen. Weitere Informationen finden Sie unter [Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App mithilfe von XAML-Inseln](host-custom-control-with-xaml-islands.md).

* In früheren Versionen dieser Anweisungen haben Sie das `maxversiontested` -Element einem Anwendungs Manifest in der WPF-oder Windows Forms-Projekt hinzugefügt. Solange Sie die neuesten Vorschau Versionen der oben aufgeführten nuget-Pakete verwenden, müssen Sie dieses Element nicht mehr dem Manifest hinzufügen.

### <a name="architecture-of-xaml-island-net-controls"></a>Architektur von .NET-Steuerelementen der XAML-Insel

Im folgenden finden Sie einen kurzen Blick darauf, wie die verschiedenen Typen von XAML-Steuerelementen auf der UWP-XAML-Hosting-API architektonisch organisiert sind.

![Hoststeuerelement-Architektur](images/xaml-islands/host-controls.png)

Die am Diagrammende aufgeführten APIs sind im Lieferumfang des Windows SDK enthalten. Die umschließenen Steuerelemente und Host Steuerelemente sind über nuget-Pakete im Windows Community Toolkit verfügbar.

### <a name="web-view-controls"></a>Webansichts Steuerelemente

Das Windows Community Toolkit bietet auch die folgenden .NET-Steuerelemente für das Hosting von Webinhalten in WPF-und Windows Forms-Anwendungen. Diese Steuerelemente werden häufig in ähnlichen Szenarien für die Desktop-App-Modernisierung verwendet wie die XAML-Insel Steuerelemente, und Sie werden im gleichen Repository von [Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) -Repository wie die XAML-Insel Steuerelemente verwaltet.

| Steuerelement | Mindestens unterstütztes Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, Version 1803 | Verwendet das Microsoft Edge-Renderingmodul, um Webinhalte anzuzeigen. |
| [Webviewcompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Bietet eine Version von **WebView** , die mit mehr Betriebssystemversionen kompatibel ist. Dieses Steuerelement verwendet das Microsoft Edge-Renderingmodul, um Webinhalte in Windows 10, Version 1803 und höher, anzuzeigen, und die Internet Explorer-Rendering-Engine zeigt Webinhalt in früheren Versionen von Windows 10, Windows 8. x und Windows 7 an. |

Wenn Sie diese Steuerelemente verwenden möchten, installieren Sie eines der folgenden nuget-Pakete:

* WPF [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Win32-Anwendungen

Die .NET-Steuerelemente der XAML-Insel C++ werden in Win32-Anwendungen nicht unterstützt. Diese Anwendungen müssen stattdessen die *UWP-XAML-Hosting-API* verwenden, die vom Windows 10 SDK bereitgestellt wird (Version 1903 und höher).

Die UWP-XAML-Hosting-API besteht aus mehreren Windows-Runtime Klassen und com C++ -Schnittstellen, mit denen die Win32-Anwendung beliebige UWP-Steuerelemente hosten kann, die von [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)abgeleitet werden. Sie können UWP-Steuerelemente in jedem Benutzeroberflächen Element in der Anwendung hosten, das über ein zugeordnetes Fenster Handle (HWND) verfügt. Weitere Informationen zu dieser API einschließlich der Voraussetzungen finden [Sie unter Verwenden der UWP-XAML-Hosting C++ -API in einer Win32-App](using-the-xaml-hosting-api.md).

> [!NOTE]
> Die umschließenden Steuerelemente und Host Steuerelemente im Windows Community Toolkit verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, das Sie andernfalls selbst behandeln müssen, wenn Sie die UWP-XAML-Hosting-API direkt verwendet haben, einschließlich der Tastaturnavigation. und Layoutänderungen. Für WPF-und Windows Forms-Anwendungen wird dringend empfohlen, dass Sie diese Steuerelemente anstelle der UWP-XAML-Hosting-API direkt verwenden, da Sie viele der Implementierungsdetails für die Verwendung der API abstrahieren.

## <a name="feature-roadmap"></a>Featureroadmap

Ab der Veröffentlichung von Windows 10, Version 1903, sind die XAML-Steuerelemente für die .net-Insel im Windows Community Toolkit weiterhin in der Developer Preview, bis die Version 1,0 der Steuerelemente verfügbar ist.

* Version 1,0 der Steuerelemente für die .NET Framework 4.6.2 und höher ist für die Veröffentlichung in der [6,0-Version des Toolkits](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)geplant.
* Version 1,0 der Steuerelemente für .net Core 3 ist für eine spätere Version des Toolkits geplant.
* Wenn Sie die neueste Vorschauversion der Versionen 1,0 der Steuerelemente für den .NET Framework und .net Core 3 testen möchten, finden Sie weitere Informationen unter den nuget-Paketen der Version 6.0.0-preview7 (oder höher) im [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) Gallery.

Weitere Informationen finden Sie unter [in diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Hintergrundinformationen und Tutorials zur Verwendung von XAML-Inseln finden Sie in den folgenden Artikeln und Ressourcen:

* [Modernisieren eines WPF-App-Tutorials](modernize-wpf-tutorial.md): Dieses Tutorial enthält schrittweise Anleitungen zum Verwenden der umschließenden Steuerelemente und Host Steuerelemente im Windows Community Toolkit zum Hinzufügen von UWP-Steuerelementen zu einer vorhandenen WPF-Branchen Anwendung. Dieses Tutorial enthält den gesamten Code für die WPF-Anwendung sowie ausführliche Anweisungen zu den einzelnen Schritten in diesem Prozess.
* [XAML-Inseln v1: Updates und Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): In diesem Blogbeitrag werden viele häufig gestellte Fragen zu XAML-Inseln erläutert und eine ausführliche Entwicklungs Roadmap bereitgestellt.
