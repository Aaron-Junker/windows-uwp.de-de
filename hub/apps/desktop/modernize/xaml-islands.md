---
description: Diese Anleitung hilft Ihnen, Fluent-basierte UWP-UIs direkt in einer WPF- oder Windows Forms-Anwendung zu erstellen.
title: Hosten von WinRT-XAML-Steuerelementen in Desktop-Apps
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 6e881fbb883d35d35b70eb8984b9264acd78191d
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270177"
---
# <a name="host-winrt-xaml-controls-in-desktop-apps-xaml-islands"></a>Hosten von WinRT-XAML-Steuerelementen in Desktop-Apps (XAML Islands)

Ab Windows 10, Version 1903, können Sie mithilfe eines Features namens *XAML Islands* WinRT-XAML-Steuerelemente auch in anderen als UWP-Desktop-Anwendungen hosten. Mit diesem Feature können Sie Aussehen, Handhabung und Funktionalität Ihrer vorhandenen WPF-, Windows Forms- und C++-Win32-Anwendungen mit den aktuellen Features der Windows 10-Benutzeroberfläche verbessern, die nur in Form von WinRT-XAML-Steuerelementen verfügbar sind. Dies bedeutet, dass Sie UWP-Features, wie etwa [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions), und Steuerelemente, die das [Fluent Design-System](/windows/uwp/design/fluent-design-system/index) unterstützen, in Ihren vorhandenen WPF-, Windows Forms- und C++-Win32-Anwendungen nutzen können.

Sie können beliebige WinRT-XAML-Steuerelemente hosten, die von [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) abgeleitet sind, darunter:

* Alle vom Originalhersteller stammenden WinRT-XAML-Steuerelemente, die im Windows SDK oder der WinUI 2.x-Bibliothek bereitstehen.
* Alle benutzerdefinierten WinRT-XAML-Steuerelemente (beispielsweise ein Benutzersteuerelement, das aus mehreren WinRT-XAML-Steuerelementen besteht, die zusammenarbeiten). Sie müssen über den Quellcode für das benutzerdefinierte Steuerelement verfügen, damit Sie es mit Ihrer Anwendung kompilieren können.

Grundsätzlich werden XAML Islands mithilfe der *UWP XAML-Hosting-API* erstellt. Diese API besteht aus mehreren Windows-Runtime Klassen und COM-Schnittstellen, die im SDK für Windows 10, Version 1903, eingeführt wurden. Wir stellen darüber hinaus eine Reihe von XAML Island-.NET-Steuerelementen im [Windows Community Toolkit](/windows/uwpcommunitytoolkit/) zur Verfügung, die die UWP XAML-Hosting-API intern verwenden und für WPF- und Windows Forms-Apps ein komfortableres Entwicklungserlebnis bieten.

Die Art, wie Sie XAML Islands verwenden, hängt von Ihrem Anwendungstyp und den Typen der WinRT-XAML-Steuerelemente ab, die Sie hosten möchten.

> [!NOTE]
> Wenn Sie Feedback zu XAML Islands haben, erstellen Sie ein neues Thema im [Microsoft.Toolkit.Win32-Repository](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues), und geben Sie dort Ihre Kommentare ab.

## <a name="requirements"></a>Anforderungen

XAML Islands hat folgende Runtimeanforderungen:

* Windows 10, Version 1903 oder höher.
* Wenn deine Anwendung für die Bereitstellung nicht in einem [MSIX-Paket](/windows/msix) gepackt ist, muss auf dem Computer die [Visual C++-Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

## <a name="wpf-and-windows-forms-applications"></a>WPF- und Windows Forms-Anwendungen

> [!NOTE]
> Die Verwendung von XAML Islands zum Hosten von WinRT-XAML-Steuerelementen in WPF- und Windows Forms-Apps wird aktuell nur in Apps für die Zielplattform .NET Core 3.x unterstützt. XAML Islands werden in Apps für die Zielplattform .NET 5 oder in Apps für eine beliebige Version von .NET Frameworks noch nicht unterstützt.

Wir empfehlen, für WPF- und Windows Forms-Anwendungen die XAML Island-.NET-Steuerelemente zu verwenden, die im Windows Community Toolkit verfügbar sind. Diese Steuerelemente bieten ein Objektmodell, das die Eigenschaften, Methoden und Ereignisse der entsprechenden WinRT-XAML-Steuerelemente nachahmt (oder Zugriff auf sie bietet). Darüber hinaus verarbeiten sie Verhalten wie Tastaturnavigation und Layoutänderungen.

Es gibt zwei Sätze von XAML Island-Steuerelementen für WPF- und Windows Forms-Anwendungen: *umschließende Steuerelemente* und *Hoststeuerelemente*. 

### <a name="wrapped-controls"></a>Umschließende Steuerelemente

WPF- und Windows Forms-Anwendungen können eine Reihe von XAML Island-Steuerelementen verwenden, die die Schnittstelle und die Funktionalität eines bestimmten WinRT-XAML-Steuerelements umschließen. Sie können diese Steuerelemente direkt der Entwurfsoberfläche Ihres WPF- oder Windows Forms-Projekts hinzufügen und sie dann wie jedes andere WPF- oder Windows Forms-Steuerelement im Designer verwenden.

Aktuell stehen die folgenden umschließenden WinRT-XAML-Steuerelemente im Windows Community Toolkit zur Verfügung. 

| Control | Unterstützte Mindestversion des Betriebssystems | Beschreibung |
|-----------------|-------------------------------|-------------|
| [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, Version 1903 | Stellen Sie eine Oberfläche und zugehörige Symbolleisten für die Windows Ink-basierte Benutzerinteraktion in Ihrer Windows Forms- oder WPF-Desktop Anwendung bereit. |
| [MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, Version 1903 | Bettet eine Ansicht ein, die Medieninhalte wie etwa Videoclips in Ihrer Windows Forms- oder WPF-Desktop-Anwendung streamt und rendert. |
| [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, Version 1903 | Ermöglicht Ihnen das Anzeigen einer symbolischen oder fotorealistischen Karte in Ihrer Windows Forms- oder WPF-Desktop-Anwendung. |

Eine exemplarische Vorgehensweise, die die Verwendung der umschließenden WinRT-XAML-Steuerelemente veranschaulicht, finden Sie unter [Hosten eines WinRT-XAML-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Hoststeuerelemente

Für benutzerdefinierte Steuerelemente und andere Szenarien, die über die mit den verfügbaren umschließenden Steuerelementen gegebenen Möglichkeiten hinausgehen, kann in WPF- und Windows Forms-Anwendungen auch das [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelement eingesetzt werden, das im Windows Community Toolkit zur Verfügung steht.

| Control | Unterstützte Mindestversion des Betriebssystems | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, Version 1903 | Kann ein beliebiges WinRT-XAML-Steuerelement hosten, das von [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) abgeleitet ist, einschließlich der vom Windows SDK bereitgestellten WinRT-XAML-Originalsteuerelemente sowie benutzerdefinierter Steuerelemente. |

Exemplarische Vorgehensweisen, in denen die Verwendung des **WindowsXamlHost**-Steuerelements veranschaulicht wird, finden Sie unter [Hosten eines WinRT-XAML-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md) und [Hosten eines benutzerdefinierten WinRT-XAML-Steuerelements in einer WPF-App mithilfe von XAML Islands](host-custom-control-with-xaml-islands.md).

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Konfigurieren des Projekts zum Verwenden der XAML Island-.NET-Steuerelemente

Für die XAML Island-.NET-Steuerelemente ist Windows 10, Version 1903, oder eine höhere Version erforderlich. Installieren Sie eins der unten aufgeführten NuGet-Pakete, um diese Steuerelemente zu verwenden. Diese Pakete bieten alles, was Sie benötigen, um die umschließenden XAML Island-Steuerelemente und die XAML Island-Hoststeuerelemente zu verwenden, und sie enthalten weitere verwandte NuGet-Pakete, die ebenfalls benötigt werden.

| Steuerelementtyp | NuGet-Paket  | Verwandte Artikel |
|-----------------|----------------|---------------------|
| [Umschließende Steuerelemente](#wrapped-controls) | Version 6.0.0 oder höher dieser Pakete: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hosten eines WinRT-XAML-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md)  |
| [Hoststeuerelement](#host-controls) | Version 6.0.0 oder höher dieser Pakete: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hosten eines WinRT-XAML-Standardsteuerelements in einer WPF-App](host-standard-control-with-xaml-islands.md)<br/>[Hosten eines benutzerdefinierten WinRT-XAML-Steuerelements in einer WPF-App](host-custom-control-with-xaml-islands.md)  |

Bitte beachten Sie die folgenden Details:

* Die Pakete mit den umschließenden Steuerelementen enthalten außerdem die Pakete mit den Hoststeuerelementen. Sie können die Pakete mit den umschließenden Steuerelementen installieren, wenn Sie beide Steuerelementsätze verwenden möchten.

* Wenn Sie ein benutzerdefiniertes WinRT-XAML-Steuerelement hosten, müssen Sie außerdem einige weitere Schritte ausführen, um auf das benutzerdefinierte Steuerelement zu verweisen. Weitere Informationen finden Sie unter [Hosten eines benutzerdefinierten WinRT-XAML-Steuerelements in einer WPF-App mithilfe von XAML Islands](host-custom-control-with-xaml-islands.md).

### <a name="web-view-controls"></a>Webansicht-Steuerelemente

Das Windows Community Toolkit stellt außerdem die folgenden .NET-Steuerelemente für das Hosten von Webinhalten in WPF- und Windows Forms-Anwendungen zur Verfügung. Diese Steuerelemente werden häufig in ähnlichen Szenarien zur Modernisierung von Desktop-Apps verwendet wie die XAML Island-Steuerelemente, und sie werden im gleichen [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)-Repository wie die XAML Island-Steuerelemente verwaltet.

| Control | Unterstützte Mindestversion des Betriebssystems | Beschreibung |
|-----------------|-------------------------------|-------------|
| [WebView](/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, Version 1803 | Verwendet die Microsoft Edge-Renderingengine zum Anzeigen von Webinhalten. |
| [WebViewCompatible](/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Bietet eine Version von **WebView**, die mit mehr Betriebssystemversionen kompatibel ist. Dieses Steuerelement verwendet die Microsoft Edge-Renderingengine, um Webinhalte unter Windows 10, Version 1803 und höher, und die Internet Explorer-Renderingengine, um Webinhalte in früheren Windows 10-Versionen sowie Windows 8.x und Windows 7 anzuzeigen. |

Installieren Sie eins dieser NuGet-Pakete, um diese Steuerelemente zu verwenden:

* WPF: [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32-Anwendungen

Die XAML Island-.NET-Steuerelemente werden in C++ Win32-Anwendungen nicht unterstützt. Diese Anwendungen müssen stattdessen die *UWP XAML-Hosting-API* verwenden, die vom Windows 10 SDK bereitgestellt wird (Version 1903 und höher).

Die UWP XAML-Hosting-API besteht aus verschiedenen Windows Runtime-Klassen und COM-Schnittstellen, die von Ihrer C++ Win32-Anwendung zum Hosten beliebiger WinRT-XAML-Steuerelemente verwendet werden können, die von [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) abgeleitet sind. WinRT-XAML-Steuerelemente lassen sich in jedem beliebigen Element der Benutzeroberfläche hosten, dem ein Fensterhandle (HWND) zugeordnet ist. Weitere Informationen zu dieser API findest du in den folgenden Artikeln.

* [Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines WinRT-XAML-Standardsteuerelements in einer C++-Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten WinRT-XAML-Steuerelements in einer C++-Win32-App](host-custom-control-with-xaml-islands-cpp.md)

> [!NOTE]
> Die umschließenden Steuerelemente und Hoststeuerelemente im Windows Community Toolkit verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, einschließlich Tastaturnavigation und Layoutänderungen, um das Sie sich andernfalls, bei direkter Verwendung der UWP XAML-Hosting-API, selbst kümmern müssten. Für WPF- und Windows Forms-Anwendungen empfehlen wir dringend, diese Steuerelemente zu nutzen, statt die UWP XAML-Hosting-API direkt zu verwenden, da sie Ihnen durch Abstraktion viele der Implementierungsdetails ersparen, die mit der Verwendung der API einhergehen.

## <a name="architecture-of-xaml-islands"></a>Architektur von XAML Islands

Hier geben wir einen kurzen Überblick zur architektonischen Gliederung der XAML Island-Steuerelemente auf dem Fundament der UWP XAML-Hosting-API.

![Hoststeuerelement-Architektur](images/xaml-islands/host-controls.png)

Die am Diagrammende aufgeführten APIs sind im Lieferumfang des Windows SDK enthalten. Die umschließenden Steuerelemente und die Hoststeuerelemente stehen im Windows Community Toolkit in Form von NuGet-Paketen zur Verfügung.

## <a name="limitations-and-workarounds"></a>Einschränkungen und Problemumgehungen

In den folgenden Abschnitten werden Einschränkungen und Problemumgehungen für bestimmte UWP-Entwicklungsszenarien bei Desktop-Apps mit XAML Islands erläutert. 

### <a name="supported-only-with-workarounds"></a>Nur mit Problemumgehungen unterstützt

:heavy_check_mark: Das Hosting von Steuerelementen aus der [WinUI 2.x-Bibliothek](../../winui/index.md) in einer XAML Islands-Instanz wird im aktuellen Release von XAML Islands bedingt unterstützt. Wenn Ihre Desktop-App ein [MSIX-Paket](/windows/msix) für die Bereitstellung verwendet, können Sie WinUI-Steuerelemente aus einer Vorabversion oder Releaseversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets hosten. Wenn Ihre Desktop-App nicht unter Verwendung von MSIX gepackt ist, können Sie WinUI-Steuerelemente nur dann hosten, wenn Sie eine Vorabversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets installieren. Unterstützung für das Hosten von Steuerelementen aus der [WinUI 3.0-Bibliothek](../../winui/winui3/index.md) wird in einem späteren Release geboten werden.

:heavy_check_mark: Um auf das Stammelement einer Struktur von XAML-Inhalten in einer XAML Islands-Instanz zuzugreifen und verwandte Informationen zum Kontext abzurufen, in dem dieser gehostet wird, solltest du nicht die Klassen [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) oder [Window](/uwp/api/windows.ui.xaml.window) verwenden. Verwende stattdessen die [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)-Klasse. Weitere Informationen finden Sie in [diesem Abschnitt](#window-host-context-for-xaml-islands).

:heavy_check_mark: Um den [Freigabe-Vertrag](/windows/uwp/app-to-app/share-data) einer WPF-, Windows Forms- oder C++-Win32-App zu unterstützen, muss deine App über Schnittstelle [IDataTransferManagerInterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) das [DataTransferManager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager)-Objekt abrufen, um den Freigabevorgang für ein bestimmtes Fenster zu initiieren. Ein Beispiel, das veranschaulicht, wie diese Schnittstelle in einer WPF-App verwendet wird, findest du im [ShareSource-Beispiel](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource).

:heavy_check_mark: Die Verwendung von `x:Bind` mit gehosteten Steuerelementen in XAML Islands wird nicht unterstützt. Du musst das Datenmodell in einer .NET Standard-Bibliothek deklarieren.

### <a name="not-supported"></a>Nicht unterstützt

:no_entry_sign: Verwenden von XAML Islands in WPF- und Windows Forms-Apps mit .NET Framework als Ziel. XAML Islands wird nur in Apps unterstützt, die auf .NET Core 3.x abzielen.

:no_entry_sign: UWP-XAML-Inhalt in XAML Islands reagiert nicht auf Windows-Designänderungen von dunkel zu hell oder umgekehrt zur Laufzeit. Der Inhalt reagiert jedoch zur Laufzeit auf Änderungen zu hohem Kontrast.

:no_entry_sign: Hinzufügen eines **WebView**-Steuerelements zu einem benutzerdefinierten Steuerelement (entweder im Thread, außerhalb des Threads oder außerhalb des Prozesses)

:no_entry_sign: Das Steuerelement [MediaPlayer](/uwp/api/Windows.Media.Playback.MediaPlayer) und das Hoststeuerelement [MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) werden im Vollbildmodus nicht unterstützt.

:no_entry_sign: Texteingabe in der Handschriftansicht. Weitere Informationen zu diesem Feature findest du in [diesem Artikel](/windows/uwp/design/controls-and-patterns/text-handwriting-view).

:no_entry_sign: Textsteuerelemente, die die Inhaltslinks `@Places` und `@People` verwenden. Weitere Informationen zu diesem Feature findest du in [diesem Artikel](/windows/uwp/design/controls-and-patterns/content-links).

:no_entry_sign: XAML-Inseln unterstützen nicht das Hosting eines [ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)-Elements, das ein Steuerelement enthält, das Texteingaben annimmt, z. B. ein [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)-, [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox)- oder [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)-Element. Wenn Sie dies verwenden, reagiert das Eingabesteuerelement auf Tastenanschläge nicht ordnungsgemäß. Um eine ähnliche Funktionalität mit einer XAML-Insel zu erreichen, empfiehlt es sich, ein [Popup](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)-Element zu hosten, das das Eingabesteuerelement enthält.

:no_entry_sign: XAML Islands-Instanzen unterstützen derzeit nicht das Anzeigen von SVG-Dateien in einem gehosteten [Windows.UI.Xaml.Controls.Image](/uwp/api/Windows.UI.Xaml.Controls.Image)-Steuerelement oder unter Verwendung eines [Windows.UI.Xaml.Media.Imaging.SvgImageSource](/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)-Objekts. Um dieses Problem zu umgehen, konvertieren Sie die Bilddateien, die Sie anzeigen möchten, in rasterbasierten Formaten wie JPG oder PNG.

### <a name="window-host-context-for-xaml-islands"></a>Fensterhostkontext für XAML Islands

Wenn Sie XAML Islands in einer Desktop-App hosten, können Sie mehrere Strukturen mit XAML-Inhalten gleichzeitig im selben Thread ausführen. Um auf das Stammelement eine Struktur von XAML-Inhalten in einer XAML Island zuzugreifen und verwandte Informationen über den Kontext abzurufen, in dem dieser gehostet wird, verwenden Sie die Klasse [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot). Die Klassen [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) und [Window](/uwp/api/windows.ui.xaml.window) liefern nicht die richtigen Informationen für XAML Islands. [CoreWindow](/uwp/api/windows.ui.core.corewindow)- und [Window](/uwp/api/windows.ui.xaml.window)-Objekte existieren zwar im Thread und sind für Ihre App zugänglich, aber sie geben keine sinnvollen Begrenzungen oder Sichtbarkeiten zurück (sie sind immer unsichtbar und haben eine Größe von 1x1). Weitere Informationen finden Sie unter [Fensterhosts](/windows/uwp/design/layout/show-multiple-views#windowing-hosts).

Um beispielsweise das Begrenzungsrechteck des Fensters abzurufen, das ein WinRT-XAML-Steuerelement enthält, das in einer XAML Island gehostet wird, verwenden Sie die Eigenschaft [XamlRoot.Size](/uwp/api/windows.ui.xaml.xamlroot.size) des Steuerelements. Da jedes WinRT-XAML-Steuerelement, das in einer XAML Island gehostet werden kann, von [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) abgeleitet wird, können Sie die [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)-Eigenschaft des Steuerelements verwenden, um auf das **XamlRoot**-Objekt zuzugreifen.

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

Verwenden Sie nicht die Eigenschaft [CoreWindows.Bounds](/uwp/api/windows.ui.core.corewindow.bounds), um das Begrenzungsrechteck abzurufen.

```csharp
// This will return incorrect information for a WinRT XAML control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

Eine Tabelle mit allgemeinen fensterbezogenen APIs, die Sie im Kontext von XAML Islands vermeiden sollten, und die empfohlenen [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)-Ersetzungen finden Sie in der Tabelle in [diesem Abschnitt](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts).

Ein Beispiel, das veranschaulicht, wie diese Schnittstelle in einer WPF-App verwendet wird, findest du im [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource)-Beispiel.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Hintergrundinformationen und Tutorials zur Verwendung von XAML Islands finden Sie in den folgenden Artikeln und Ressourcen:

* [Tutorial Modernisieren einer WPF-App](modernize-wpf-tutorial.md): Dieses Tutorial enthält schrittweise Anleitungen zum Verwenden der umschließenden Steuerelemente und Hoststeuerelemente im Windows Community Toolkit zum Hinzufügen von WinRT-XAML-Steuerelementen zu einer vorhandenen WPF-Branchenanwendung. Dieses Tutorial enthält den vollständigen Code für die WPF-Anwendung sowie ausführliche Anweisungen zu den einzelnen Schritten in diesem Prozess.
* [Codebeispiele für XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples): Dieses Repository enthält Windows Forms-, WPF- und C++/Win32-Beispiele, die die Verwendung von XAML-Inseln demonstrieren.
* [XAML Islands v1 – Updates and Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): In diesem Blogbeitrag werden viele häufig gestellte Fragen zu XAML Islands erläutert und eine ausführliche Entwicklungsroadmap zur Verfügung gestellt.