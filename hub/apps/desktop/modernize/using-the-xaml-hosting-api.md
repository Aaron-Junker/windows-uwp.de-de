---
description: In diesem Artikel wird das Hosten der UWP XAML-Benutzeroberfläche in C++-Win32-Desktop-Apps beschrieben.
title: Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32,XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80218460"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App

Ab Windows 10, Version 1903, können nicht auf UWP basierende Desktop-Apps (einschließlich C++-Win32-, WPF- und Windows Forms-Apps) die *UWP XAML-Hosting-API* verwenden, um UWP-Steuerelemente in beliebigen Elementen der Benutzeroberfläche hosten, die einem Fensterhandle (HWND) zugeordnet sind. Diese API ermöglicht es nicht auf der Universellen Windows Plattform basierenden Desktop-Apps, die neuesten Features der Windows 10-Benutzeroberfläche zu verwenden, die nur in Form von UWP-Steuerelementen verfügbar sind. Beispielsweise können nicht auf UWP basierende Desktop-Apps diese API nutzen, um UWP-Steuerelemente zu hosten, die das [Fluent Design System](/windows/uwp/design/fluent-design-system/index) verwenden und [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) unterstützen.

Die UWP XAML-Hosting-API bildet die Grundlage für eine umfassenderen Satz von Steuerelementen, die wir bereitstellen, um Entwicklern die Verwendung der Fluent-Benutzeroberfläche in nicht auf UWP basierenden Desktop-Apps zu ermöglichen. Dieses Feature wird als *XAML Islands* bezeichnet. Eine Übersicht zu diesem Feature finden Sie unter [Hosten von UWP XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md).

> [!NOTE]
> Wenn Sie Feedback zu XAML Islands haben, erstellen Sie ein neues Thema im [Microsoft.Toolkit.Win32-Repository](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues), und geben Sie dort Ihre Kommentare ab. Wenn Sie Ihr Feedback lieber privat geben möchten, können Sie es an XamlIslandsFeedback@microsoft.com senden. Ihre Erkenntnisse und Szenarien sind äußerst wichtig für uns.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>Ist die UWP XAML-Hosting-API die richtige Wahl für Ihre Desktop-App?

Die UWP XAML-Hosting-API stellt die Low-Level-Infrastruktur zum Hosten von UWP-Steuerelementen in Desktop-Apps bereit. Einige Arten von Desktop-Apps können alternative, komfortablere APIs verwenden, um dieses Ziel zu erreichen.

* Wenn Sie über eine C++ Win32-Desktop-App verfügen und UWP-Steuerelemente in Ihrer APP hosten möchten, müssen Sie die UWP XAML-Hosting-API verwenden. Für diese Art von Apps gibt es keine Alternativen.

* Für WPF- und Windows Forms-Apps empfehlen wir dringend die Verwendung der [XAML Islands .NET-Steuerelemente](xaml-islands.md#wpf-and-windows-forms-applications) im Windows Community Toolkit, statt die UWP XAML-Hosting-API direkt zu verwenden. Diese Steuerelemente verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, einschließlich Tastaturnavigation und Layoutänderungen, um das Sie sich andernfalls, bei direkter Verwendung der UWP XAML-Hosting-API, selbst kümmern müssten.

Da wir die Verwendung der UWP XAML-Hosting-API nur für C++-Win32-Apps empfehlen, bietet dieser Artikel in erster Linie Anweisungen und Beispiele für C++-Win32-Apps. Wenn Sie es wünschen, können Sie die UWP XAML-Hosting-API jedoch in WPF- und Windows Forms-Apps verwenden. Dieser Artikel verweist auf den relevanten Quellcode für die [Hoststeuerelemente](xaml-islands.md#host-controls) für WPF und Windows Forms im Windows Community Toolkit, damit Sie sehen können, wie die UWP XAML-Hosting-API von diesen Steuerelementen verwendet wird.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Die Verwendung der XAML-Hosting-API

Lesen Sie diese Artikel, um den schrittweisen Anweisungen zur Verwendung der XAML-Hosting-API in C++-Win32-Apps zu folgen:

* [Hosten eines UWP-Standardsteuerelements](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten UWP-Steuerelements](host-custom-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Beispiele

Die Art und Weise, in der Sie die UWP XAML-Hosting-API in Ihrem Code verwenden, hängt vom Typ Ihrer App, dem Entwurf Ihrer App und weiteren Faktoren ab. Um zu veranschaulichen, wie diese API im Kontext einer kompletten App verwendet werden kann, bezieht sich dieser Artikel auf Code aus den folgenden Beispielen.

### <a name="c-win32"></a>C++ Win32

In den folgenden Beispielen wird gezeigt, wie die UWP XAML-Hosting-API in einer C++-Win32-App verwendet wird:

* [Einfaches Beispiel für XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Dieses Beispiel veranschaulicht eine einfache Hostingimplementierung eines UWP-Steuerelements in einer nicht verpackten C++-Win32-App (d. h. einer App, die nicht in einem MSIX-Paket verpackt ist).

* [Beispiel für XAML Island mit benutzerdefiniertem Steuerelement](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Dieses Beispiel zeigt eine vollständige Hostingimplementierung für ein benutzerdefiniertes UWP-Steuerelement in einer verpackten C++-Win32-App sowie die Handhabung anderer Verhaltensweisen wie Tastatureingaben und Fokusnavigation.

### <a name="wpf-and-windows-forms"></a>WPF und Windows Forms

Das [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelement im Windows-Community-Toolkit dient als Referenzbeispiel zur Verwendung der UWP-Hosting-API in WPF- und Windows Forms-Apps. Sie finden den Quellcode an folgenden Speicherorten:

* Die WPF-Version des Steuerelements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Die WPF-Version ist von [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) abgeleitet.

* Die Windows Forms-Version des Steuerelements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Die Windows Forms-Version ist von [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) abgeleitet.

> [!NOTE]
> Wir empfehlen dringend, in WPF- und Windows Forms-Apps die [XAML Islands .NET-Steuerelemente](xaml-islands.md#wpf-and-windows-forms-applications) im Windows-Community-Toolkit zu verwenden, statt die UWP XAML-Hosting-API direkt zu verwenden. Die Links zu den WPF- und Windows Forms-Beispielen in diesem Artikel dienen nur der Veranschaulichung.

## <a name="architecture-of-the-api"></a>Architektur der API

Die UWP XAML-Hosting-API umfasst diese Windows Runtime-Haupttypen und -COM-Schnittstellen.

|  Typ oder Schnittstelle | Beschreibung |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Diese Klasse stellt das UWP-XAML-Framework dar. Diese Klasse stellt eine einzelne statische [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)-Methode bereit, die das UWP XAML-Framework im aktuellen Thread in der Desktop-App initialisiert. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Diese Klasse stellt eine Instanz von UWP-XAML-Inhalt dar, den Sie in Ihrer Desktop-App hosten. Das wichtigste Mitglied dieser Klasse ist die [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)-Eigenschaft. Sie weisen diese Eigenschaft einem [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) zu, das Sie hosten möchten. Diese Klasse weist außerdem andere Elemente zum Routen der Tastaturfokusnavigation in und aus XAML Islands auf. |
| IDesktopWindowXamlSourceNative | Diese COM-Schnittstelle stellt die **AttachToWindow**-Methode bereit, die Sie verwenden, um ein XAML Island in Ihrer App an ein übergeordnetes Benutzeroberflächenelement anzufügen. Diese Schnittstelle wird von jedem **DesktopWindowXamlSource**-Objekt implementiert. Diese Schnittstelle ist in windows.ui.xaml.hosting.desktopwindowxamlsource.h implementiert. |
| IDesktopWindowXamlSourceNative2 | Diese COM-Schnittstelle stellt die **PreTranslateMessage**-Methode bereit, die dem UWP XAML-Framework die ordnungsgemäße Behandlung bestimmter Windows-Nachrichten ermöglicht. Diese Schnittstelle wird von jedem **DesktopWindowXamlSource**-Objekt implementiert. Diese Schnittstelle ist in windows.ui.xaml.hosting.desktopwindowxamlsource.h implementiert. |

Das folgende Diagramm veranschaulicht die Hierarchie der Objekte in einem XAML Island, das in einer Desktop-App gehostet wird.

* Auf der Basisebene befindet sich das UI-Element Ihrer App, in dem Sie das XAML Island hosten möchten. Dieses UI-Element muss über ein Fensterhandle (HWND) verfügen. Beispiele für Benutzeroberflächenelemente, in denen ein XAML Island gehostet werden kann, sind ein [Fenster](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) in C++-Win32-Apps, ein [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) in WPF-Apps und ein [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) in Windows Forms-Apps.

* Auf der nächsten Ebene befindet sich ein **DesktopWindowXamlSource**-Objekt. Dieses Objekt stellt die Infrastruktur zum Hosten des XAML Islands bereit. Ihr Code muss dieses Objekt erstellen und es an das übergeordnete Benutzeroberflächenelement anfügen.

* Wenn Sie ein **DesktopWindowXamlSource** erstellen, erstellt dieses Objekt automatisch ein natives untergeordnetes Fenster zum Hosten des UWP-Steuerelements. Das native untergeordnete Fenster ist größtenteils von Ihrem Code abstrahiert, Sie können bei Bedarf aber auf sein Handle (HWND) zugreifen.

* Auf der obersten Ebene befindet sich schließlich das UWP-Steuerelement, das Sie in Ihrer Desktop-App hosten möchten. Dabei kann es sich um ein beliebiges UWP-Objekt handeln, das von [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) abgeleitet ist, einschließlich aller UWP-Steuerelemente, die vom Windows SDK bereitgestellt werden, sowie benutzerdefinierter Steuerelemente.

![DesktopWindowXamlSource-Architektur](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Wenn Sie XAML Islands in einer Desktop-App hosten, können Sie mehrere Strukturen mit XAML-Inhalten gleichzeitig im selben Thread ausführen. Um auf das Stammelement eine Struktur von XAML-Inhalten in einer XAML Island zuzugreifen und verwandte Informationen über den Kontext abzurufen, in dem dieser gehostet wird, verwenden Sie die Klasse [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Die APIs [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) und [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) liefern nicht die richtigen Informationen für XAML Islands. Weitere Informationen finden Sie in [diesem Abschnitt](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Fehler beim Verwenden der UWP XAML-Hosting-API in einer UWP-App

| Problem | Lösung |
|-------|------------|
| Ihre App empfängt eine **COMException** mit der folgenden Nachricht: „DesktopWindowXamlSource kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden“. oder „WindowsXamlManager kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden“. | Dieser Fehler weist darauf hin, dass Sie versuchen, die UWP XAML-Hosting-API in einer UWP-App zu verwenden (insbesondere versuchen Sie, die Typen [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) oder [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) zu instanziieren). Die UWP XAML-Hosting-API ist nur zur Verwendung in nicht auf UWP basierenden Desktop-Apps bestimmt, wie etwa WPF-, Windows Forms- und C++-Win32-Anwendungen. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Fehler beim Versuch, die Typen WindowsXamlManager oder DesktopWindowXamlSource zu verwenden

| Problem | Lösung |
|-------|------------|
| Ihre App empfängt eine Ausnahme mit der folgenden Nachricht: „WindowsXamlManager und DesktopWindowXamlSource werden für Apps mit der Zielplattform Windows, Version 10.0.18226.0 und höher, unterstützt. Überprüfen Sie entweder das Anwendungsmanifest oder das Paketmanifest, und vergewissern Sie sich, dass die MaxTestedVersion-Eigenschaft aktualisiert wurde“. | Dieser Fehler weist darauf hin, dass Ihre Anwendung versucht hat, die Typen **WindowsXamlManager** oder **DesktopWindowXamlSource** in der UWP XAML-Hosting-API zu verwenden, das Betriebssystem aber nicht bestimmen kann, ob die App für die Zielplattform Windows 10, Version 1903 oder höher, erstellt wurde. Die UWP XAML-Hosting-API wurde zunächst als Vorschauversion in einer früheren Version von Windows 10 vorgestellt, sie wird aber erst ab Windows 10, Version 1903, unterstützt.</p></p>Um dieses Problem zu beheben, erstellen Sie entweder ein MSIX-Paket für die App, und führen Sie sie aus diesem Paket aus, oder installieren Sie das NuGet-Paket [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) in Ihrem Projekt.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Fehler beim Anfügen an ein Fenster in einem anderen Thread

| Problem | Lösung |
|-------|------------|
| Ihre App empfängt eine **COMException** mit der folgenden Nachricht: „Fehler bei der AttachToWindow-Methode, weil das angegebene HWND in einem anderen Thread erstellt wurde“. | Dieser Fehler weist darauf hin, dass Ihre Anwendung die **IDesktopWindowXamlSourceNative::AttachToWindow**-Methode aufgerufen und ihr das HWND eines Fensters übergeben hat, das in einem anderen Thread erstellt wurde. Sie müssen dieser Methode das HWND eines Fensters übergeben, das im gleichen Thread wie der Code erstellt wurde, von dem aus Sie die Methode aufrufen. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Fehler beim Anfügen an ein Fenster eines anderen Fensters der obersten Ebene

| Problem | Lösung |
|-------|------------|
| Ihre App empfängt eine **COMException** mit der folgenden Nachricht: „Fehler bei der AttachToWindow-Methode, weil das angegebene HWND von einem anderen Fenster der obersten Ebene stammt als das HWND, das AttachToWindow zuvor im gleichen Thread übergeben wurde“. | Dieser Fehler weist darauf hin, dass Ihre Anwendung die **IDesktopWindowXamlSourceNative::AttachToWindow**-Methode aufgerufen und ihr das HWND eines Fensters übergeben hat, das von einem anderen Fenster der obersten Ebene stammt als ein Fenster, das Sie in einem früheren Aufruf dieser Methode im gleichen Thread angegeben haben.</p></p>Nachdem Ihre Anwendung **AttachToWindow** in einem bestimmten Thread aufgerufen hat, können alle anderen [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)-Objekte im gleichen Thread nur an Fenster angefügt werden, die Nachfolger des gleichen Fensters der obersten Ebene sind, das im ersten Aufruf von **AttachToWindow** übergeben wurde. Wenn alle **DesktopWindowXamlSource**-Objekte für einen bestimmten Thread geschlossen sind, kann das nächste **DesktopWindowXamlSource** wieder an ein beliebiges Fenster angefügt werden.</p></p>Um dieses Problem zu beheben, schließen Sie entweder alle **DesktopWindowXamlSource**-Objekte, die in diesem Thread an andere Fenster der obersten Ebene gebunden sind, oder erstellen Sie einen neuen Thread für dieses **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Zugehörige Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md)
* [Hosten eines UWP-Standardsteuerelements in einer C++-Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App](host-custom-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien für XAML Islands in C++-Win32-Apps](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands-Codebeispiele](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++-Win32-XAML Islands-Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
