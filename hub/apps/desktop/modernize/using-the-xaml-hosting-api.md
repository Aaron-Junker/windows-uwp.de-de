---
description: In diesem Artikel wird beschrieben, wie Sie die XAML-Benutzeroberfläche C++ von UWP in Ihrer Desktop-Win32-App
title: Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, XAML-Inseln
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218460"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App

Ab Windows 10, Version 1903, können nicht-UWP-Desktop-Apps C++ (einschließlich Win32-, WPF-und Windows Forms-Apps) die *UWP-XAML-Hosting-API* verwenden, um UWP-Steuerelemente in jedem Benutzeroberflächen Element zu hosten, das einem Fenster Handle (HWND) zugeordnet ist. Diese API ermöglicht es nicht-UWP-Desktop-Apps, die neuesten Windows 10-Benutzeroberflächen Features zu verwenden, die nur über UWP-Steuerelemente verfügbar sind. Beispielsweise können nicht-UWP-Desktop-Apps diese API verwenden, um UWP-Steuerelemente zu hosten, die das [fließende Design System](/windows/uwp/design/fluent-design-system/index) verwenden und [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)unterstützen.

Die UWP-XAML-Hosting-API stellt die Grundlage für einen umfassenderen Satz von Steuerelementen dar, die wir bereitstellen, damit Entwickler eine fließende Benutzeroberfläche für Desktop-Apps ohne UWP bereitstellen können. Diese Funktion wird als *XAML-Inseln*bezeichnet. Eine Übersicht über diese Funktion finden Sie unter [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md).

> [!NOTE]
> Wenn Sie Feedback zu XAML Islands haben, erstellen Sie ein neues Thema im [Microsoft.Toolkit.Win32-Repository](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues), und geben Sie dort Ihre Kommentare ab. Wenn Sie Ihr Feedback lieber privat geben möchten, können Sie es an XamlIslandsFeedback@microsoft.com senden. Ihre Erkenntnisse und Szenarien sind äußerst wichtig für uns.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>Ist die UWP-XAML-Hosting-API die richtige Wahl für Ihre Desktop-App?

Die UWP-XAML-Hosting-API bietet die Low-Level-Infrastruktur zum Hosten von UWP-Steuerelementen in Desktop-Apps. Einige Arten von Desktop-Apps können alternative, bequemere APIs verwenden, um dieses Ziel zu erreichen.

* Wenn Sie über eine C++ Win32-Desktop-App verfügen und UWP-Steuerelemente in Ihrer APP hosten möchten, müssen Sie die UWP-XAML-Hosting-API verwenden. Für diese Art von apps gibt es keine Alternativen.

* Für WPF-und Windows Forms-apps wird dringend empfohlen, dass Sie die [XAML](xaml-islands.md#wpf-and-windows-forms-applications) -Steuerelemente für die Insel .net im Windows Community Toolkit verwenden, anstatt die UWP-XAML-Hosting-API direkt zu verwenden. Diese Steuerelemente verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, das Sie andernfalls selbst behandeln müssen, wenn Sie die UWP-XAML-Hosting-API direkt verwendet haben, einschließlich der Tastaturnavigation und der Layoutänderungen.

Wir empfehlen, dass nur C++ Win32-Apps die UWP-XAML-Hosting-API verwenden. dieser Artikel enthält Haupt C++ sächlich Anweisungen und Beispiele für Win32-apps. Sie können jedoch die UWP-XAML-Hosting-API in WPF und Windows Forms Apps verwenden, wenn Sie auswählen. Dieser Artikel verweist auf den relevanten Quellcode für die [Host Steuerelemente](xaml-islands.md#host-controls) für WPF und Windows Forms im Windows Community Toolkit, damit Sie sehen können, wie die UWP-XAML-Hosting-API von diesen Steuerelementen verwendet wird.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Erfahren Sie, wie Sie die XAML-Hosting-API verwenden.

In den folgenden Artikeln finden Sie Schritt-für-Schritt-Anleitungen mit Codebeispielen für die Verwendung der C++ XAML-Hosting-API in Win32-apps:

* [Hosten eines UWP-Standard Steuer Elements](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten UWP-Steuer Elements](host-custom-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Beispiele

Die Art und Weise, in der Sie die UWP-XAML-Hosting-API in Ihrem Code verwenden, hängt von Ihrem App-Typ, dem Design Ihrer APP und anderen Faktoren ab. Um zu veranschaulichen, wie diese API im Kontext einer kompletten App verwendet werden kann, bezieht sich dieser Artikel auf Code aus den folgenden Beispielen.

### <a name="c-win32"></a>C++Win32

In den folgenden Beispielen wird gezeigt, wie die UWP-XAML-Hosting C++ -API in einer Win32-App verwendet wird:

* [Einfaches Beispiel für eine XAML-Insel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Dieses Beispiel veranschaulicht eine grundlegende Implementierung des Hostings eines UWP-Steuer Elements C++ in einer nicht verpackten Win32-app (d. h. eine APP, die nicht in ein msix-Paket integriert ist).

* [XAML-Insel mit benutzerdefiniertem Steuerelement Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Dieses Beispiel veranschaulicht eine komplette Implementierung des Hostings eines benutzerdefinierten UWP-Steuer C++ Elements in einer APP mit gepackten Win32-App sowie die Handhabung anderer Verhaltensweisen wie Tastatureingaben und Fokus Navigation.

### <a name="wpf-and-windows-forms"></a>WPF und Windows Forms

Das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement im Windows Community Toolkit dient als Referenzbeispiel für die Verwendung der UWP-Hosting-API in WPF-und Windows Forms-apps. Der Quellcode ist an den folgenden Speicherorten verfügbar:

* Die WPF-Version des Steuer Elements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Die WPF-Version wird von [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)abgeleitet.

* Die Windows Forms-Version des Steuer Elements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Die Windows Forms Version wird von [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)abgeleitet.

> [!NOTE]
> Es wird dringend empfohlen, die [XAML](xaml-islands.md#wpf-and-windows-forms-applications) -Steuerelemente für die Insel .net im Windows Community Toolkit zu verwenden, anstatt die UWP-XAML-Hosting-API direkt in WPF-und Windows Forms-apps zu verwenden. Die Links zu den WPF-und Windows Forms Beispielen in diesem Artikel dienen lediglich der Veranschaulichung.

## <a name="architecture-of-the-api"></a>Architektur der API

Die UWP-XAML-Hosting-API umfasst diese Haupt Windows-Runtime Typen und COM-Schnittstellen.

|  Typ oder Schnittstelle | Beschreibung |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Diese Klasse stellt das UWP-XAML-Framework dar. Diese Klasse stellt eine einzelne statische [initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) -Methode bereit, die das UWP-XAML-Framework auf dem aktuellen Thread in der Desktop-App initialisiert. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Diese Klasse stellt eine Instanz des UWP-XAML-Inhalts dar, den Sie in Ihrer Desktop-App gehostet haben. Der wichtigste Member dieser Klasse ist die [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) -Eigenschaft. Sie weisen diese Eigenschaft einem [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) zu, das Sie hosten möchten. Diese Klasse verfügt auch über andere Member zum Routing der Tastaturfokus Navigation in und aus den XAML-Inseln. |
| Idesktopwindowxamlsourcenative | Diese COM-Schnittstelle stellt die **attachdewindow** -Methode bereit, mit der Sie eine XAML-Insel in der APP an ein übergeordnetes Benutzeroberflächen Element anfügen können. Jedes **desktopwindowxamlsource** -Objekt implementiert diese Schnittstelle. Diese Schnittstelle wird in Windows. UI. XAML. Hosting. desktopwindowxamlsource. h deklariert. |
| IDesktopWindowXamlSourceNative2 | Diese COM-Schnittstelle stellt die **pretranslatemess** -Methode bereit, die es dem UWP-XAML-Framework ermöglicht, bestimmte Windows-Meldungen ordnungsgemäß zu verarbeiten. Jedes **desktopwindowxamlsource** -Objekt implementiert diese Schnittstelle. Diese Schnittstelle wird in Windows. UI. XAML. Hosting. desktopwindowxamlsource. h deklariert. |

Das folgende Diagramm veranschaulicht die Hierarchie von Objekten in einer XAML-Insel, die in einer Desktop-App gehostet wird.

* Auf der Basisebene befindet sich das UI-Element in Ihrer APP, in dem Sie die XAML-Insel hosten möchten. Dieses UI-Element muss über ein Fenster Handle (HWND) verfügen. Beispiele für Elemente der Benutzeroberfläche, in denen Sie eine XAML-Insel [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) hosten C++ können, sind ein Fenster für Win32-apps, ein [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) für WPF-apps und ein [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) für Windows Forms-apps.

* Auf der nächsten Ebene befindet sich ein **desktopwindowxamlsource** -Objekt. Dieses Objekt stellt die Infrastruktur zum Hosting der XAML-Insel bereit. Der Code ist dafür verantwortlich, dieses Objekt zu erstellen und an das übergeordnete UI-Element anzufügen.

* Wenn Sie eine **desktopwindowxamlsource**erstellen, erstellt dieses Objekt automatisch ein natives untergeordnetes Fenster, um das UWP-Steuerelement zu hosten. Dieses systemeigene untergeordnete Fenster wird größtenteils aus dem Code abstrahiert, aber Sie können ggf. auf das Handle (HWND) zugreifen.

* Auf der obersten Ebene befindet sich schließlich das UWP-Steuerelement, das Sie in Ihrer Desktop-App hosten möchten. Dabei kann es sich um ein beliebiges UWP-Objekt handeln, das von " [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)" abgeleitet wird, einschließlich sämtlicher UWP-Steuerelemente, die von der Windows SDK bereitgestellt werden, sowie

![Desktopwindowxamlsource-Architektur](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Wenn Sie XAML Islands in einer Desktop-App hosten, können Sie mehrere Strukturen mit XAML-Inhalten gleichzeitig im selben Thread ausführen. Um auf das Stammelement eine Struktur von XAML-Inhalten in einer XAML Island zuzugreifen und verwandte Informationen über den Kontext abzurufen, in dem dieser gehostet wird, verwenden Sie die Klasse [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Die [corewindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)-, [applicationview](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)-und [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) -APIs bieten keine korrekten Informationen für XAML-Inseln. Weitere Informationen finden Sie in [diesem Abschnitt](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Fehler beim Verwenden der UWP-XAML-Hosting-API in einer UWP-App

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "desktopwindowxamlsource kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden. " oder "windowsxamlmanager kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden. " | Dieser Fehler weist darauf hin, dass Sie versuchen, die UWP-XAML-Hosting-API (insbesondere die Typen " [desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) " oder " [windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ") in einer UWP-APP zu verwenden. Die UWP-XAML-Hosting-API ist nur für die Verwendung in Desktop-Apps ohne UWP (z. b. WPF C++ -, Windows Forms-und Win32-Anwendungen) vorgesehen. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Fehler beim Versuch, die Typen "windowsxamlmanager" oder "desktopwindowxamlsource" zu verwenden.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine Ausnahme mit folgender Meldung: "windowsxamlmanager und desktopwindowxamlsource werden für Apps unterstützt, die auf Windows-Version 10.0.18226.0 und höher abzielen. Überprüfen Sie entweder das Anwendungs Manifest oder das Paket Manifest, und stellen Sie sicher, dass die maxtestedversion-Eigenschaft aktualisiert wird. " | Dieser Fehler zeigt an, dass die Anwendung versucht hat, die Typen **windowsxamlmanager** oder **desktopwindowxamlsource** in der UWP-XAML-Hosting-API zu verwenden, aber das Betriebssystem kann nicht bestimmen, ob die APP für das Ziel Windows 10, Version 1903 oder höher, entwickelt wurde. Die UWP-XAML-Hosting-API wurde erstmals als Vorschauversion in einer früheren Version von Windows 10 eingeführt, wird jedoch nur ab Windows 10, Version 1903, unterstützt.</p></p>Um dieses Problem zu beheben, erstellen Sie entweder ein msix-Paket für die APP, führen Sie es aus dem Paket aus, oder installieren Sie das nuget-Paket [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) in Ihrem Projekt.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Fehler beim Anfügen an ein Fenster in einem anderen Thread.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "die attachdewindow-Methode konnte nicht ausgeführt werden, da das angegebene HWND in einem anderen Thread erstellt wurde". | Dieser Fehler weist darauf hin, dass die Anwendung die **idesktopwindowxamlsourcenative:: attachdewindow** -Methode aufgerufen hat und ihr das HWND eines Fensters, das in einem anderen Thread erstellt wurde, weitergereicht hat. Sie müssen diese Methode als HWND eines Fensters übergeben, das im gleichen Thread wie der Code erstellt wurde, von dem aus Sie die-Methode aufrufen. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Fehler beim Anfügen an ein Fenster in einem anderen Fenster der obersten Ebene.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "die attachdewindow-Methode konnte nicht ausgeführt werden, da das angegebene HWND von einem anderen Fenster der obersten Ebene abgeleitet ist als das HWND, das zuvor an attachtewindow im gleichen Thread weitergegeben wurde." | Dieser Fehler zeigt an, dass die Anwendung die **idesktopwindowxamlsourcenative:: attachdewindow** -Methode aufgerufen und das HWND eines Fensters an das Fenster weitergeleitet hat, das von einem anderen Fenster der obersten Ebene abgeleitet ist als ein Fenster, das Sie in einem vorherigen Aufruf dieser Methode im gleichen Thread angegeben haben.</p></p>Nachdem die Anwendung " **attachdewindow** " für einen bestimmten Thread aufgerufen hat, können alle anderen [desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) -Objekte im gleichen Thread nur an Fenster angefügt werden, die nachfolgende Elemente desselben Fensters der obersten Ebene sind, das beim ersten Aufruf von " **AttachTo Window**" weitergegeben wurde. Wenn alle **desktopwindowxamlsource** -Objekte für einen bestimmten Thread geschlossen sind, kann die nächste **desktopwindowxamlsource** wieder an jedes Fenster angefügt werden.</p></p>Um dieses Problem zu beheben, schließen Sie entweder alle **desktopwindowxamlsource** -Objekte, die an andere Fenster der obersten Ebene in diesem Thread gebunden sind, oder erstellen Sie einen neuen Thread für diese **desktopwindowxamlsource**. |

## <a name="related-topics"></a>Verwandte Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md)
* [Hosten eines UWP-Standard Steuer C++ Elements in einer Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten UWP- C++ Steuer Elements in einer Win32-App](host-custom-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps](advanced-scenarios-xaml-islands-cpp.md)
* [Codebeispiele für XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Beispiel für Win32-XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
