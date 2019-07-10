---
description: Diese Anleitung hilft Ihnen, Fluent-basierte UWP-UIs direkt in einer WPF- oder Windows Forms-Anwendung zu erstellen.
title: UWP-Steuerelementen in desktop-apps
ms.date: 04/16/2019
ms.topic: article
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8ceb314424ae2611e141ef866a84c08e55b0ba2d
ms.sourcegitcommit: f9a30bfd1e8eab50d0b1db97dd2f650ce66b5d34
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690888"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Host UWP XAML-Steuerelemente in desktop-apps (XAML-Inseln)

Ab Windows 10, Version 1903 sein, können Sie UWP-Steuerelementen in nicht-UWP-desktopanwendungen, die mit einem Feature hosten *XAML-Inseln*. Dieses Feature ermöglicht Sie erhöhen die aussehen, Verhalten und Funktionalität von Ihren bestehenden desktopanwendungen mit den neuesten Windows 10-Benutzeroberflächen-Features, die nur über die UWP-Steuerelemente verfügbar sind. Dies bedeutet, dass Sie UWP-Features wie z. B. können [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) und Steuerelemente, unterstützen die [Fluent-Entwurfssystem](/windows/uwp/design/fluent-design-system/index) in Ihre vorhandenen WPF, Windows Forms und C++-Win32-Anwendungen.

Wir bieten mehrere Möglichkeiten, verwenden in Ihrer Windows Forms, WPF-XAML-Inseln und C++ Win32-Anwendungen abhängig von der Technologie oder ein Framework, die Sie verwenden. 

> [!NOTE]
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem in der [Microsoft.Toolkit.Win32 Repository](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) und lassen Sie Ihre Kommentare vorhanden. Wenn Sie Ihr Feedback zu senden, privat möchten, können Sie senden, damit XamlIslandsFeedback@microsoft.com. Ihre Einblicke und Szenarien sind sehr wichtig für uns.

## <a name="how-do-xaml-islands-work"></a>Wie funktionieren die Inseln für XAML?

Ab Windows 10, Version 1903 sein, wir bieten zwei Möglichkeiten zur Verwendung in Ihrer Windows Forms, WPF-XAML-Inseln und C++ Win32-Anwendungen:

* Das Windows SDK enthält mehrere Windows-Runtime-Klassen und COM-Schnittstellen, die Ihre Anwendung verwenden können, um alle UWP-Steuerelemente hosten, die von abgeleitet [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Zusammen werden diese Klassen und Schnittstellen bezeichnet die *UWP XAML hosting-API*, und bieten die Möglichkeit zum Hosten von UWP-Steuerelementen in einem UI-Element in der Anwendung, die über eine zugeordnete Fenster-Handle (HWND) verfügt. Weitere Informationen zu dieser API finden Sie unter [mithilfe der hosting-API XAML](using-the-xaml-hosting-api.md).

* Die [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) enthält auch zusätzliche Steuerelemente der XAML-Insel für WPF und Windows Forms. Diese Steuerelemente verwenden die UWP XAML intern hosting-API und implementieren das Verhalten, die Sie andernfalls benötigen würden, um selbst zu behandeln, wenn Sie auf der hosting-API direkt, einschließlich Tastatur Navigation und das Layout Änderungen UWP XAML verwendet. Für WPF und Windows Forms-Anwendungen wird dringend empfohlen, dass Sie diese Steuerelemente verwenden, anstatt die UWP XAML direkt hosting-API, da sie entfernt viele Implementierungsdetails der Verwendung der API abstrahieren. Beachten Sie, dass die ab Windows 10, Version 1903 sein, diese Steuerelemente sind [verfügbar als Entwicklervorschau](#feature-roadmap).

> [!NOTE]
> C++-Win32-desktop-Anwendungen müssen die UWP XAML hosting-API zum Hosten von UWP-Steuerelementen verwenden. Die XAML-Island-Steuerelemente in das Windows-Community-Toolkit sind nicht verfügbar für C++ Win32-desktopanwendungen.

Es gibt zwei Arten von XAML Island-Steuerelementen, die von der Windows-Community-Toolkit für WPF- und Windows Forms-Anwendungen bereitgestellt werden: *umschlossen Steuerelemente* und *Hoststeuerelemente*. 

### <a name="wrapped-controls"></a>Umschlossene Steuerelemente

WPF- und Windows Forms-Anwendungen können eine Auswahl von umschlossenen UWP-Steuerelemente in der [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Diese heißen *umschlossen Steuerelemente* , da sie die Schnittstelle und die Funktionalität eines bestimmten Steuerelements UWP umschließen. Sie können diese Steuerelemente direkt auf die Entwurfsoberfläche des WPF oder Windows Forms-Projekts hinzufügen und verwenden Sie diese dann wie jedes andere WPF oder Windows Forms-Steuerelement im Designer.

Die folgenden umschlossene UWP-Steuerelemente für die Implementierung der XAML-Inseln sind derzeit für WPF- und Windows Forms-Anwendungen verfügbar.

| Steuerelement | Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, Version 1903 | Geben Sie eine Fläche aus, und verwandte Symbolleisten für Windows Ink-basierte Benutzerinteraktion in Ihre Windows Forms oder WPF-Desktopanwendung an. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, Version 1903 | Bettet eine Ansicht, die streams auf und rendert die Medieninhalte wie z. B. Videos in Ihre Windows Forms oder WPF-Desktopanwendung an. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, Version 1903 | Können Sie eine symbolische oder fotorealistische-Karte in Ihrer Windows Forms oder WPF-Desktopanwendung anzeigen. |

Zusätzlich zu den umschlossenen Steuerelementen für XAML-Inseln bietet das Windows-Community-Toolkit auch die folgenden Steuerelemente für das Hosten von Web-Inhalte.

| Steuerelement | Betriebssystem | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, Version 1803 | Verwendet die Microsoft Edge-Rendering-Engine Webinhalt angezeigt. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Bietet eine Version von **WebView** mit mehr OS-Versionen kompatibel ist. Dieses Steuerelement verwendet wird, die Microsoft Edge-Rendering-Engine Webinhalt auf Windows 10, Version 1803 und höher angezeigt und das Internet Explorer-Renderingmodul anzuzeigenden Webinhalten in früheren Versionen von Windows 10, Windows 8.x und Windows 7. |

### <a name="host-controls"></a>Hosten von Steuerelementen

Für Szenarien durch die verfügbaren Steuerelemente für die umschlossene abgedeckt, WPF- und Windows Forms-Anwendungen können auch die [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) steuern, der [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Dieses Steuerelement kann alle UWP-Steuerelemente, die abgeleitet hosten [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), einschließlich alle UWP-Steuerelement, die vom Windows SDK bereitgestellt werden sowie benutzerdefinierte Benutzersteuerelemente. Dieses Steuerelement unterstützt die Windows 10 Insider Preview SDK, Build, 17709 und höhere Versionen.

### <a name="architecture-overview"></a>Übersicht über die Architektur

Nachfolgend ist dargestellt, wie diese Steuerelemente architektonisch organisiert sind.

![Hoststeuerelement-Architektur](images/xaml-islands/host-controls.png)

Die am Diagrammende aufgeführten APIs sind im Lieferumfang des Windows SDK enthalten. Der umschlossene Steuerelementen und Hoststeuerelementen stehen über Nuget-Pakete in das Windows-Community-Toolkit.

## <a name="requirements"></a>Anforderungen

XAML-Inseln erfordern Windows 10, Version 1903 und höher. Um XAML-Inseln in Ihrer Anwendung verwenden zu können, müssen Sie zuerst das Projekt einrichten.

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>Schritt 1: Ändern Sie das Projekt zur Verwendung von Windows-Runtime-APIs

Anweisungen hierzu finden Sie unter [in diesem Artikel](desktop-to-uwp-enhance.md#set-up-your-project).

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>Schritt 2: Enable XAML Island-Unterstützung in Ihrem Projekt

Stellen Sie eine der folgenden Änderungen auf Ihr Projekt, damit der XAML-Dateninsel unterstützt. Weitere Informationen finden Sie unter [in diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117).

#### <a name="option-1-package-your-application-in-an-msix-package"></a>Option 1: Packen der Anwendung in einem Paket MSIX  

Installieren Sie das Windows 10, die Version 1903 SDK (oder eine höhere Version). Klicken Sie dann zum Packen der Anwendung in einem Paket MSIX durch Hinzufügen einer [Paketerstellungsprojekt für Windows-Anwendungen](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) zu Ihrer Projektmappe einen Verweis auf Ihre WPF- oder Windows Forms-Projekt hinzufügen.

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>Option 2: Legen Sie den Wert "maxversiontested" in Ihrem Assemblymanifest

Wenn Sie nicht Ihre Anwendung in einem Paket MSIX packen möchten, können Sie hinzufügen ein [Anwendungsmanifest](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) zu Ihrem Projekt und fügen die **"maxversiontested"** Elements im Manifest an, dass Ihre Anwendung ist kompatibel mit Windows 10, Version 1903 oder höher.

1. Wenn Sie noch nicht vorhanden ist eine Anwendung ein manifest in Ihrem Projekt das Projekt eine neue XML-Datei hinzu, und nennen Sie sie **"App.manifest"** . Für eine WPF oder Windows Forms-Anwendung, stellen Sie sicher, dass Sie auch Zuweisen der **Manifest** Eigenschaft **. "App.manifest"** in die **Anwendung** auf der Seite Ihrer [Projekt Eigenschaften](https://docs.microsoft.com/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources).
2. Im Anwendungsmanifest, enthalten die **Kompatibilität** Element und die untergeordneten Elemente, die im folgenden Beispiel gezeigt. Ersetzen Sie die **Id** Attribut der **"maxversiontested"** Element mit der Versionsnummer von Windows 10, die Sie Anzielen (Dies muss Windows 10, Version 1903 sein oder eine höhere Version sein).

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
            <application>
                <!-- Windows 10 -->
                <maxversiontested Id="10.0.18362.0"/>
                <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
            </application>
        </compatibility>
    </assembly>
    ```

> [!NOTE]
> Beim Hinzufügen einer **"maxversiontested"** Element um ein Anwendungsmanifest in einem C++ Win32 Projekt (mit einer Projektvorlage für Windows Desktop-Anwendung in Visual Studio), die Sie möglicherweise die folgende Warnung angezeigt, in Ihrem Projekt: `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"` . Diese Warnung, nicht dass nichts falsch. in Ihrem Projekt ist, und er ignoriert werden kann.

## <a name="feature-roadmap"></a>Roadmap für die Funktion

Ab der Version von Windows 10, Version 1903 sein, sind die umschlossene Steuerelementen und Hoststeuerelementen im Windows-Community-Toolkit immer noch in einer entwicklervorschauversion bis der Veröffentlichung von Version 1.0 der Steuerelemente zur Verfügung steht.

* Version 1.0 der Steuerelemente für .NET Framework 4.6.2 und höher in freigegeben werden geplant werden die [Version 6.0 des Toolkits](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* Version 1.0 der Steuerelemente für .NET Core-3 werden für ein späteres Release des Toolkits geplant.
* Wenn Sie sicher diese neuesten Previews der Version 1.0 Versionen dieser Steuerelemente für die .NET Framework und .NET Core-3 testen möchten, finden Sie unter den **6.0.0-preview3** NuGet-Pakete der [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) Katalog.

Weitere Informationen finden Sie unter [in diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen und Lernprogramme zur Verwendung von XAML-Inseln finden Sie unter den folgenden Artikeln und Ressourcen:

* [Modernisieren Sie ein WPF-app-Tutorial](modernize-wpf-tutorial.md): In diesem Tutorial enthält schrittweise Anleitungen zum Verwenden der umschlossene Steuerelementen und Hoststeuerelementen im Windows-Community-Toolkit UWP-Steuerelementen zu einer vorhandenen WPF-Line-of-Business-Anwendung hinzufügen. Dieses Lernprogramm enthält den vollständigen Code für die WPF-Anwendung sowie detaillierte Anweisungen für jeden Schritt im Prozess an.
* [XAML-Inseln v1 - Updates und Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): In diesem Blogbeitrag erläutert, häufige gestellte Fragen zur XAML-Inseln und bietet eine detaillierte Entwicklungsplan.
