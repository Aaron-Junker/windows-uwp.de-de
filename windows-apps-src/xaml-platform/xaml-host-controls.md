---
description: Diese Anleitung hilft Ihnen, Fluent-basierte UWP-UIs direkt in einer WPF- oder Windows Forms-Anwendung zu erstellen.
title: UWP-Steuerelemente in Desktopanwendungen
ms.date: 01/11/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619705"
---
# <a name="uwp-controls-in-desktop-applications"></a>UWP-Steuerelemente in Desktopanwendungen

> [!NOTE]
> XAML-Inseln sind derzeit als Entwicklervorschau verfügbar. Obwohl Sie diese in Ihrem eigenen Prototypcode ausprobieren können, jetzt bald, empfehlen wir nicht, die Sie im Produktionscode zu diesem Zeitpunkt verwenden. Diese APIs und Steuerelementen weiterhin, heranzureifen und stabilisieren in zukünftigen Versionen von Windows. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.
>
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem in der [WindowsCommunityToolkit Repository](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) und lassen Sie Ihre Kommentare vorhanden. Wenn Sie Ihr Feedback zu senden, privat möchten, können Sie senden, damit XamlIslandsFeedback@microsoft.com. Ihre Einblicke und Szenarien sind sehr wichtig für uns.

Windows 10 ermöglicht jetzt das UWP-Steuerelementen in nicht-UWP-desktop-Anwendungen zu verwenden, sodass Sie verbessern können, die aussehen, Verhalten und Funktionalität von Ihren bestehenden desktopanwendungen mit den neuesten Windows 10-Benutzeroberflächen-Features, die nur über die UWP-Steuerelemente verfügbar sind. Dies bedeutet, dass Sie UWP-Features wie z. B. können [Windows Ink](../design/input/pen-and-stylus-interactions.md) und Steuerelemente, unterstützen die [Fluent-Entwurfssystem](../design/fluent-design-system/index.md) in Ihre vorhandenen WPF, Windows Forms und C++-Win32-Anwendungen. Dieses Szenario Developer bezeichnet *XAML-Inseln*.

Wir bieten verschiedene Möglichkeiten zum Verwenden von XAML-Inseln in Ihren WPF, Windows Forms und C++-Win32-Anwendungen, abhängig von der Technologie oder ein Framework, die Sie verwenden.

## <a name="wrapped-controls"></a>Umschlossene Steuerelemente

WPF- und Windows Forms-Anwendungen können eine Auswahl von umschlossenen UWP-Steuerelemente in der [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Wir bezeichnen diese Steuerelemente als *umschlossen Steuerelemente* , da sie die Schnittstelle und die Funktionalität eines bestimmten Steuerelements UWP umschließen. Sie können diese Steuerelemente direkt auf die Entwurfsoberfläche des WPF oder Windows Forms-Projekts hinzufügen und verwenden Sie diese dann wie jedes andere WPF oder Windows Forms-Steuerelement im Designer.

> [!NOTE]
> Umschlossene Steuerelemente sind nicht für C++-Win32-desktopanwendungen verfügbar. Diese Typen von Anwendungen verwenden, müssen die [UWP XAML hosting-API](#uwp-xaml-hosting-api).

Die folgenden umschlossene UWP-Steuerelemente sind derzeit für WPF- und Windows Forms-Anwendungen verfügbar. Weitere umschlossen UWP-Steuerelemente sind für zukünftige Versionen des Windows-Community Toolkits geplant.

| Steuerelement | Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, Version 1803 | Verwendet die Microsoft Edge-Rendering-Engine Webinhalt angezeigt. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Bietet eine Version von **WebView** mit mehr OS-Versionen kompatibel ist. Dieses Steuerelement verwendet wird, die Microsoft Edge-Rendering-Engine Webinhalt auf Windows 10, Version 1803 und höher angezeigt und das Internet Explorer-Renderingmodul anzuzeigenden Webinhalten in früheren Versionen von Windows 10, Windows 8.x und Windows 7. |
| [InkCanvas-Steuerelement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, Version 1809 (build 17763) | Geben Sie eine Fläche aus, und verwandte Symbolleisten für Windows Ink-basierte Benutzerinteraktion in Ihre Windows Forms oder WPF-Desktopanwendung an. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, Version 1809 (build 17763) | Bettet eine Ansicht, die streams auf und rendert die Medieninhalte wie z. B. Videos in Ihre Windows Forms oder WPF-Desktopanwendung an. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, Version 1809 (build 17763) | Können Sie eine symbolische oder fotorealistische-Karte in Ihrer Windows Forms oder WPF-Desktopanwendung anzeigen. |

## <a name="host-controls"></a>Hosten von Steuerelementen

Für Szenarien durch die verfügbaren Steuerelemente für die umschlossene abgedeckt, WPF- und Windows Forms-Anwendungen können auch die [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) steuern, der [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Dieses Steuerelement kann alle UWP-Steuerelemente, die abgeleitet hosten [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), einschließlich alle UWP-Steuerelement, die vom Windows SDK bereitgestellt werden sowie benutzerdefinierte Benutzersteuerelemente. Dieses Steuerelement unterstützt die Windows 10 Insider Preview SDK, Build, 17709 und höhere Versionen.

> [!NOTE]
> Hoststeuerelemente sind nicht für C++-Win32-desktopanwendungen verfügbar. Diese Typen von Anwendungen verwenden, müssen die [UWP XAML hosting-API](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>UWP XAML hosting-API

Wenn Sie eine C++-Win32-Anwendung verfügen, können Sie mithilfe der *UWP XAML hosting-API* auf alle UWP-Steuerelemente hosten, die von abgeleitet [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) in jedem Element der Benutzeroberfläche in Ihre Anwendung mit einer zugeordneten Fensterhandle (HWND). Diese API wurde in Windows 10 Insider Preview SDK Build 17709 eingeführt. Weitere Informationen zur Verwendung dieser API finden Sie unter [mithilfe der hosting-API in einer Desktopanwendung XAML](using-the-xaml-hosting-api.md).

> [!NOTE]
> C++-Win32-desktop-Anwendungen müssen die UWP XAML hosting-API zum Hosten von UWP-Steuerelementen verwenden. Umschlossene Steuerelemente und-Hoststeuerelemente sind nicht verfügbar für diese Arten von Anwendungen. Für WPF und Windows Forms-Anwendungen empfehlen wir die Verwendung der umschlossene Steuerelementen und Hoststeuerelementen im Windows-Community-Toolkit anstelle der UWP XAML hosting-API werden. Diese Steuerelemente verwenden Sie die UWP XAML intern hosting-API und eine einfachere Entwicklungsumgebung bereitzustellen. Allerdings können Sie die UWP XAML hosting-API direkt in WPF- und Windows Forms-Anwendungen, wenn Sie auswählen.

## <a name="architecture-overview"></a>Übersicht über die Architektur

Nachfolgend ist dargestellt, wie diese Steuerelemente architektonisch organisiert sind. Die in diesem Diagramm verwendeten Namen werden möglicherweise noch geändert.  

![Hoststeuerelement-Architektur](images/host-controls.png)

Die am Diagrammende aufgeführten APIs sind im Lieferumfang des Windows SDK enthalten. Die Steuerelemente für den Designer sind als Nuget-Pakete im Windows Community Toolkit geliefert enthalten.

Diese neuen Steuerelemente haben noch Einschränkungen. Bevor Sie die Steuerelemente verwenden, sollten Sie sich einen Moment Zeit nehmen und lesen, was noch nicht unterstützt wird oder nur mit Problemumgehungen funktioniert.

## <a name="limitations"></a>Einschränkungen

### <a name="whats-supported"></a>Nicht unterstützt

In den meisten Fällen wird nur die Funktionalität nicht unterstützt, die in der folgenden Liste aufgeführt ist.

### <a name="whats-supported-only-with-workarounds"></a>Nur mit Problemumgehungen unterstützt

:heavy_check_mark: Hosten von mehreren Posteingang-Steuerelementen in mehrere Fenster. Sie müssen jedes Fenster in seinem eigenen Thread platzieren.

:heavy_check_mark: Mithilfe von ``x:Bind`` Steuerelemente gehostet. Sie müssen das Datenmodell in einer .NET Standardbibliothek deklarieren.

:heavy_check_mark: C#-basierte Steuerelemente eines Drittanbieters. Wenn Sie über den Quellcode eines Drittanbieter-Steuerelements verfügen, können Sie ihn kompilieren.

### <a name="whats-not-yet-supported"></a>Noch nicht unterstützt

: No_entry_sign: Tools für Barrierefreiheit, die funktionieren nahtlos in die Anwendung und die gehosteten Steuerelemente.

: No_entry_sign: Lokalisierte Inhalte in Steuerelementen, die Sie Anwendungen hinzufügen, die ein Windows-app-Paket nicht enthalten.

: No_entry_sign: Objektverweise in XAML vorgenommen werden, in Anwendungen, die ein Windows-app-Paket nicht enthalten.

: No_entry_sign: Steuerelemente, die ordnungsgemäß reagieren auf Änderungen der DPI-Wert und Skalierung.

: No_entry_sign: Hinzufügen einer **WebView** Steuerelement ein benutzerdefiniertes Steuerelement, das (auf Thread, aus Threads, oder aus der Prozedur).

: No_entry_sign: Die [anzeigen Hervorhebung](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent wirksam.

: No_entry_sign: Inline Freihand, @Places, und @People für Eingabesteuerelemente.

: No_entry_sign: Zuweisen von Zugriffstasten.

: No_entry_sign: C++-basierte Drittanbieter-Steuerelementen.

: No_entry_sign: Hosten von benutzerdefinierten Steuerelementen.

Die Elemente in dieser Liste werden sich wahrscheinlich ändern, da wir ständig daran arbeiten, Fluent auf dem Desktop zu optimieren.  
