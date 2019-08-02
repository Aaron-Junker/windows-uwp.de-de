---
description: Diese Anleitung hilft Ihnen, Fluent-basierte UWP-UIs direkt in einer WPF- oder Windows Forms-Anwendung zu erstellen.
title: UWP-Steuerelemente in Desktop-Apps
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 765fefa0b489e1620d7a37fe75acd02acb8d5ae8
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729479"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)

Ab Windows 10, Version 1903, können Sie UWP-Steuerelemente in nicht-UWP-Desktop Anwendungen mit einer Funktion namens " *XAML-Inseln*" hosten. Diese Funktion ermöglicht es Ihnen, das Aussehen, das Gefühl und die Funktionalität Ihrer vorhandenen Desktop Anwendungen mit den neuesten Windows 10-Benutzeroberflächen Features zu verbessern, die nur über UWP-Steuerelemente zur Verfügung stehen. Dies bedeutet, dass Sie UWP-Funktionen wie z. b. [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) und Steuerelemente verwenden können, die das [fließende Entwurfs System](/windows/uwp/design/fluent-design-system/index) in Ihren vorhandenen C++ WPF-, Windows Forms-und Win32-Anwendungen unterstützen.

Es gibt mehrere Möglichkeiten, XAML-Inseln in Ihren WPF-, Windows Forms- C++ und Win32-Anwendungen zu verwenden. Dies hängt von der verwendeten Technologie oder dem Framework ab. 

> [!NOTE]
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem im Repository " [Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) ", und lassen Sie Ihre Kommentare dort ablegen. Wenn Sie Ihr Feedback lieber privat einreichen möchten, können Sie es an XamlIslandsFeedback@microsoft.comsenden. Ihre Einblicke und Szenarios sind für uns äußerst wichtig.

## <a name="how-do-xaml-islands-work"></a>Funktionsweise von XAML-Inseln

Ab Windows 10, Version 1903, bieten wir zwei Möglichkeiten, um XAML-Inseln in Ihren WPF-, Windows Forms- C++ und Win32-Anwendungen zu verwenden:

* Der Windows SDK stellt mehrere Windows-Runtime Klassen und COM-Schnittstellen bereit, die Ihre Anwendung verwenden kann, um beliebige UWP-Steuerelemente zu hosten, die von [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)abgeleitet werden. Gemeinsam werden diese Klassen und Schnittstellen als *UWP-XAML-Hosting-API*bezeichnet und ermöglichen Ihnen das Hosten von UWP-Steuerelementen in jedem Benutzeroberflächen Element in der Anwendung, das über ein zugeordnetes Fenster Handle (HWND) verfügt. Weitere Informationen zu dieser API finden Sie unter [Verwenden der XAML-Hosting-API](using-the-xaml-hosting-api.md).

* Das [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) bietet auch zusätzliche XAML-Insel Steuerelemente für WPF und Windows Forms. Diese Steuerelemente verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, das Sie andernfalls selbst behandeln müssen, wenn Sie die UWP-XAML-Hosting-API direkt verwendet haben, einschließlich der Tastaturnavigation und der Layoutänderungen. Für WPF-und Windows Forms-Anwendungen wird dringend empfohlen, dass Sie diese Steuerelemente anstelle der UWP-XAML-Hosting-API direkt verwenden, da Sie viele der Implementierungsdetails für die Verwendung der API abstrahieren. Beachten Sie, dass diese Steuerelemente ab Windows 10, Version 1903, [als Entwicklervorschau verfügbar](#feature-roadmap)sind.

> [!NOTE]
> C++Win32-Desktop Anwendungen müssen die UWP-XAML-Hosting-API zum Hosten von UWP-Steuerelementen verwenden. Die Steuerelemente der XAML-Insel im Windows Community Toolkit sind für C++ Win32-Desktop Anwendungen nicht verfügbar.

Es gibt zwei Typen von XAML-Insel Steuerelementen, die vom Windows Community Toolkit für WPF und Windows Forms Anwendungen bereitgestellt werden: umschließende Steuer *Elemente* und *Host Steuerelemente*. 

### <a name="wrapped-controls"></a>Umschließt Steuerelemente

WPF-und Windows Forms Anwendungen können eine Auswahl von umschließenden UWP-Steuerelementen im [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)verwenden. Diese werden als umschließende Steuer *Elemente* bezeichnet, da Sie die Schnittstelle und Funktionalität eines bestimmten UWP-Steuer Elements umschließen. Sie können diese Steuerelemente direkt zur Entwurfs Oberfläche Ihres WPF-oder Windows Forms Projekts hinzufügen und diese dann wie jedes andere WPF-oder Windows Forms-Steuerelement im Designer verwenden.

Die folgenden umschließbaren UWP-Steuerelemente für die Implementierung von XAML-Inseln sind zurzeit für WPF verfügbar (siehe das Paket " [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) ") und Windows Forms Anwendungen (siehe das [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) -Paket). ).

| Steuerelement | Mindestens unterstütztes Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, Version 1903 | Stellen Sie eine Oberfläche und zugehörige Symbolleisten für die Windows Ink-basierte Benutzerinteraktion in Ihrer Windows Forms-oder WPF-Desktop Anwendung bereit. |
| [Mediaplayerelement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, Version 1903 | Bettet eine Ansicht ein, die Medieninhalte wie z. b. Videos in Ihrer Windows Forms-oder WPF-Desktop Anwendung streamt und rendert |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, Version 1903 | Ermöglicht es Ihnen, eine symbolische oder eine Fotoansicht in der Windows Forms-oder WPF-Desktop Anwendung anzuzeigen. |

Zusätzlich zu den umschließenden Steuerelementen für XAML-Inseln bietet das Windows Community Toolkit auch die folgenden Steuerelemente für das Hosting von Webinhalten in WPF (siehe das Paket " [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView) ") und Windows Forms Anwendungen (siehe Das [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView) -Paket).

| Steuerelement | Mindestens unterstütztes Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, Version 1803 | Verwendet das Microsoft Edge-Renderingmodul, um Webinhalte anzuzeigen. |
| [Webviewcompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Bietet eine Version von **WebView** , die mit mehr Betriebssystemversionen kompatibel ist. Dieses Steuerelement verwendet das Microsoft Edge-Renderingmodul, um Webinhalte in Windows 10, Version 1803 und höher, anzuzeigen, und die Internet Explorer-Rendering-Engine zeigt Webinhalt in früheren Versionen von Windows 10, Windows 8. x und Windows 7 an. |

### <a name="host-controls"></a>Host Steuerelemente

Für Szenarien, die über diejenigen hinausgehen, die von den verfügbaren umschlenenen Steuerelementen abgedeckt werden, können WPF-und Windows Forms Anwendungen auch das Steuerelement [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) im [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) Dieses Steuerelement kann ein beliebiges UWP-Steuerelement hosten, das von " [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)" abgeleitet wird, einschließlich sämtlicher UWP-Steuerelemente, die von der Windows SDK bereitgestellt werden, sowie Dieses Steuerelement unterstützt Windows 10 Insider Preview SDK Build 17709 und spätere Versionen.

Diese Steuerelemente sind in den Paketen [Microsoft. Toolkit. WPF. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (für WPF) und [Microsoft. Toolkit. Forms. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (für Windows Forms) verfügbar. Diese Pakete sind in den Paketen [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) und [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) enthalten, die die umschließenden Steuerelemente enthalten.

### <a name="architecture-overview"></a>Übersicht über die Architektur

Nachfolgend ist dargestellt, wie diese Steuerelemente architektonisch organisiert sind.

![Hoststeuerelement-Architektur](images/xaml-islands/host-controls.png)

Die am Diagrammende aufgeführten APIs sind im Lieferumfang des Windows SDK enthalten. Die umschließenen Steuerelemente und Host Steuerelemente sind über nuget-Pakete im Windows Community Toolkit verfügbar.

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>Konfigurieren des Projekts für die Verwendung von XAML-Inseln

XAML-Inseln erfordern Windows 10, Version 1903 und höher. Um XAML-Inseln in Ihrer Anwendung zu verwenden, müssen Sie zuerst das Projekt einrichten:

1. Ändern Sie das Projekt, um Windows-Runtime-APIs zu verwenden. Anweisungen hierzu finden Sie in [diesem Artikel](desktop-to-uwp-enhance.md#set-up-your-project).
2. Installieren Sie eines dieser nuget-Pakete in Ihrem Projekt. Stellen Sie sicher, dass Sie Version 6.0.0-preview7 oder eine höhere Version des Pakets installieren.
    * WPF Installieren Sie [Microsoft. Toolkit. WPF. UI. Controls.](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)
    * Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)
    * C++Win32 [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)

> [!NOTE]
> In früheren Versionen dieser Anweisungen haben Sie das **maxversiongetestete** -Element einem Anwendungs Manifest in Ihrem Projekt hinzugefügt. Ab der aktuellen Vorschauversion der nuget-Pakete müssen Sie dieses Element nicht mehr dem Manifest hinzufügen.

## <a name="feature-roadmap"></a>Featureroadmap

Ab der Veröffentlichung von Windows 10, Version 1903, sind die umschpackten Steuerelemente und Host Steuerelemente im Windows Community Toolkit weiterhin in der Developer Preview, bis die Version 1,0 der Steuerelemente verfügbar ist.

* Version 1,0 der Steuerelemente für die .NET Framework 4.6.2 und höher ist für die Veröffentlichung in der [6,0-Version des Toolkits](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)geplant.
* Version 1,0 der Steuerelemente für .net Core 3 ist für eine spätere Version des Toolkits geplant.
* Wenn Sie die neueste Vorschauversion der Versionen 1,0 der Steuerelemente für den .NET Framework und .net Core 3 testen möchten, lesen Sie die **6.0.0-preview7** nuget-Pakete im [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) Gallery.

Weitere Informationen finden Sie unter [in diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Hintergrundinformationen und Tutorials zur Verwendung von XAML-Inseln finden Sie in den folgenden Artikeln und Ressourcen:

* [Modernisieren eines WPF-App-Tutorials](modernize-wpf-tutorial.md): Dieses Tutorial enthält schrittweise Anleitungen zum Verwenden der umschließenden Steuerelemente und Host Steuerelemente im Windows Community Toolkit zum Hinzufügen von UWP-Steuerelementen zu einer vorhandenen WPF-Branchen Anwendung. Dieses Tutorial enthält den gesamten Code für die WPF-Anwendung sowie ausführliche Anweisungen zu den einzelnen Schritten in diesem Prozess.
* [XAML-Inseln v1: Updates und Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): In diesem Blogbeitrag werden viele häufig gestellte Fragen zu XAML-Inseln erläutert und eine ausführliche Entwicklungs Roadmap bereitgestellt.
