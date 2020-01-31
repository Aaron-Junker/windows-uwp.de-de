---
description: In diesem Artikel wird beschrieben, wie Sie die XAML-Benutzeroberfläche C++ von UWP in Ihrer Desktop-Win32-App
title: Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, XAML-Inseln
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7574fb5920433f894819ffd3d94e31fef03d30b3
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814030"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App

Ab Windows 10, Version 1903, können nicht-UWP-Desktop-Apps C++ (einschließlich Win32-, WPF-und Windows Forms-Apps) die *UWP-XAML-Hosting-API* verwenden, um UWP-Steuerelemente in jedem Benutzeroberflächen Element zu hosten, das einem Fenster Handle (HWND) zugeordnet ist. Diese API ermöglicht es nicht-UWP-Desktop-Apps, die neuesten Windows 10-Benutzeroberflächen Features zu verwenden, die nur über UWP-Steuerelemente verfügbar sind. Beispielsweise können nicht-UWP-Desktop-Apps diese API verwenden, um UWP-Steuerelemente zu hosten, die das [fließende Design System](/windows/uwp/design/fluent-design-system/index) verwenden und [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)unterstützen.

Die UWP-XAML-Hosting-API stellt die Grundlage für einen umfassenderen Satz von Steuerelementen dar, die wir bereitstellen, damit Entwickler eine fließende Benutzeroberfläche für Desktop-Apps ohne UWP bereitstellen können. Diese Funktion wird als *XAML-Inseln*bezeichnet. Eine Übersicht über diese Funktion finden Sie unter [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md).

> [!NOTE]
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem im Repository " [Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) ", und lassen Sie Ihre Kommentare dort ablegen. Wenn Sie Ihr Feedback lieber privat einreichen möchten, können Sie es an XamlIslandsFeedback@microsoft.comsenden. Ihre Einblicke und Szenarios sind für uns äußerst wichtig.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Sollten Sie die UWP-XAML-Hosting-API verwenden?

Die UWP-XAML-Hosting-API bietet die Low-Level-Infrastruktur zum Hosten von UWP-Steuerelementen in Desktop-Apps. Einige Arten von Desktop-Apps können alternative, bequemere APIs verwenden, um dieses Ziel zu erreichen.  

* Wenn Sie über eine C++ Win32-Desktop-App verfügen und UWP-Steuerelemente in Ihrer APP hosten möchten, müssen Sie die UWP-XAML-Hosting-API verwenden. Für diese Art von apps gibt es keine Alternativen.

* Für WPF-und Windows Forms-apps wird dringend empfohlen, dass Sie die [XAML](xaml-islands.md#wpf-and-windows-forms-applications) -Steuerelemente für die Insel .net im Windows Community Toolkit verwenden, anstatt die UWP-XAML-Hosting-API direkt zu verwenden. Diese Steuerelemente verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, das Sie andernfalls selbst behandeln müssen, wenn Sie die UWP-XAML-Hosting-API direkt verwendet haben, einschließlich der Tastaturnavigation und der Layoutänderungen.

Wir empfehlen, dass nur C++ Win32-Apps die UWP-XAML-Hosting-API verwenden. dieser Artikel enthält Haupt C++ sächlich Anweisungen und Beispiele für Win32-apps. Sie können jedoch die UWP-XAML-Hosting-API in WPF und Windows Forms Apps verwenden, wenn Sie auswählen. Dieser Artikel verweist auf den relevanten Quellcode für die [Host Steuerelemente](xaml-islands.md#host-controls) für WPF und Windows Forms im Windows Community Toolkit, damit Sie sehen können, wie die UWP-XAML-Hosting-API von diesen Steuerelementen verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

XAML-Inseln erfordern Windows 10, Version 1903 (oder höher) und den entsprechenden Build der Windows SDK. Um XAML-Inseln in der C++ Win32-APP zu verwenden, müssen Sie zuerst das Projekt einrichten.

### <a name="support-for-cwinrt"></a>Unterstützung C++für/WinRT

Stellen Sie sicher, dass Ihr Projekt [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)unterstützt:

* Bei neuen Projekten können Sie die [ C++/WinRT Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installieren und eine der C++/WinRT-Projektvorlagen verwenden, die in dieser Erweiterung enthalten sind.
* Bei vorhandenen Projekten können Sie das nuget-Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) im Projekt installieren.

Weitere Informationen zu diesen Optionen finden Sie in [diesem Artikel](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="configure-your-project-for-app-deployment"></a>Konfigurieren des Projekts für die APP-Bereitstellung

Wählen Sie eine der folgenden Optionen aus, um Ihr Projekt für die Bereitstellung vorzubereiten:

* **Verpacken Sie Ihre APP in einem msix-Paket**. Das Verpacken Ihrer APP in einem [msix-Paket](https://docs.microsoft.com/windows/msix/) bietet zahlreiche Vorteile für Bereitstellung und Laufzeit.
    1. Installieren Sie das SDK für Windows 10, Version 1903 (Version 10.0.18362) oder eine spätere Version.
    2. Verpacken Sie Ihre APP in einem msix-Paket, indem Sie der Projekt Mappe ein [Windows-Anwendungs](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) Paket hinzufügen und C++einen Verweis auf das/Win32-Projekt hinzufügen.

* **Installieren Sie das Paket "Microsoft. Toolkit. Win32. UI. SDK**". Wenn Sie Ihre APP nicht in einem msix-Paket verpacken möchten, können Sie [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (Version v 6.0.0 oder höher) installieren. Dieses Paket enthält mehrere Build-und Lauf Zeit Objekte, die es ermöglichen, dass XAML-Inseln in der App funktionieren.

> [!NOTE]
> In früheren Versionen dieser Anweisungen haben Sie das `maxversiontested`-Element einem Anwendungs Manifest in Ihrem Projekt hinzugefügt. Solange Sie eine der oben aufgeführten Optionen verwenden, müssen Sie dieses Element nicht mehr dem Manifest hinzufügen.

### <a name="additional-requirements-for-custom-uwp-controls"></a>Zusätzliche Anforderungen für benutzerdefinierte UWP-Steuerelemente

Wenn Sie ein benutzerdefiniertes UWP-Steuerelement (z. b. ein Benutzer Steuerelement, das aus mehreren UWP-Steuerelementen besteht, die zusammenarbeiten) gehostet haben, müssen Sie die zusätzlichen Anweisungen in [diesem Abschnitt](#host-a-custom-uwp-control)befolgen. Sie müssen auch über den Quellcode für das benutzerdefinierte Steuerelement verfügen, damit Sie es mit Ihrer APP kompilieren können.

## <a name="architecture-of-the-api"></a>Architektur der API

Die UWP-XAML-Hosting-API umfasst diese Haupt Windows-Runtime Typen und COM-Schnittstellen.

|  Typ oder Schnittstelle | Beschreibung |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Diese Klasse stellt das UWP-XAML-Framework dar. Diese Klasse stellt eine einzelne statische [initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) -Methode bereit, die das UWP-XAML-Framework auf dem aktuellen Thread in der Desktop-App initialisiert. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Diese Klasse stellt eine Instanz des UWP-XAML-Inhalts dar, den Sie in Ihrer Desktop-App gehostet haben. Der wichtigste Member dieser Klasse ist die [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) -Eigenschaft. Sie weisen diese Eigenschaft einem [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) zu, das Sie hosten möchten. Diese Klasse verfügt auch über andere Member zum Routing der Tastaturfokus Navigation in und aus den XAML-Inseln. |
| Idesktopwindowxamlsourcenative | Diese COM-Schnittstelle stellt die **attachdewindow** -Methode bereit, mit der Sie eine XAML-Insel in der APP an ein übergeordnetes Benutzeroberflächen Element anfügen können. Jedes **desktopwindowxamlsource** -Objekt implementiert diese Schnittstelle. |
| IDesktopWindowXamlSourceNative2 | Diese COM-Schnittstelle stellt die **pretranslatemess** -Methode bereit, die es dem UWP-XAML-Framework ermöglicht, bestimmte Windows-Meldungen ordnungsgemäß zu verarbeiten. Jedes **desktopwindowxamlsource** -Objekt implementiert diese Schnittstelle. |

Das folgende Diagramm veranschaulicht die Hierarchie von Objekten in einer XAML-Insel, die in einer Desktop-App gehostet wird.

* Auf der Basisebene befindet sich das UI-Element in Ihrer APP, in dem Sie die XAML-Insel hosten möchten. Dieses UI-Element muss über ein Fenster Handle (HWND) verfügen. Beispiele für Elemente der Benutzeroberfläche, in denen Sie eine XAML-Insel [fenster](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) hosten C++ können, sind ein Fenster für Win32-apps, ein [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) für WPF-apps und ein [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) für Windows Forms-apps.

* Auf der nächsten Ebene befindet sich ein **desktopwindowxamlsource** -Objekt. Dieses Objekt stellt die Infrastruktur zum Hosting der XAML-Insel bereit. Der Code ist dafür verantwortlich, dieses Objekt zu erstellen und an das übergeordnete UI-Element anzufügen.

* Wenn Sie eine **desktopwindowxamlsource**erstellen, erstellt dieses Objekt automatisch ein natives untergeordnetes Fenster, um das UWP-Steuerelement zu hosten. Dieses systemeigene untergeordnete Fenster wird größtenteils aus dem Code abstrahiert, aber Sie können ggf. auf das Handle (HWND) zugreifen.

* Auf der obersten Ebene befindet sich schließlich das UWP-Steuerelement, das Sie in Ihrer Desktop-App hosten möchten. Dabei kann es sich um ein beliebiges UWP-Objekt handeln, das von " [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)" abgeleitet wird, einschließlich sämtlicher UWP-Steuerelemente, die von der Windows SDK bereitgestellt werden, sowie

![Desktopwindowxamlsource-Architektur](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>Verwandte Beispiele

Die Art und Weise, in der Sie die UWP-XAML-Hosting-API in Ihrem Code verwenden, hängt von Ihrem App-Typ, dem Design Ihrer APP und anderen Faktoren ab. Um zu veranschaulichen, wie diese API im Kontext einer kompletten App verwendet werden kann, bezieht sich dieser Artikel auf Code aus den folgenden Beispielen.

### <a name="c-win32"></a>C++Win32

In den folgenden Beispielen wird gezeigt, wie die UWP-XAML-Hosting C++ -API in einer Win32-App verwendet wird:

* [Einfaches Beispiel für eine XAML-Insel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Dieses Beispiel veranschaulicht eine grundlegende Implementierung des Hostings eines UWP-Steuer Elements C++ in einer nicht verpackten Win32-app (d. h. eine APP, die nicht in ein msix-Paket integriert ist).

* [XAML-Insel mit benutzerdefiniertem Steuerelement Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Dieses Beispiel veranschaulicht eine komplette Implementierung des Hostings eines benutzerdefinierten UWP-Steuer C++ Elements in einer APP mit gepackten Win32-App sowie die Handhabung anderer Verhaltensweisen wie Tastatureingaben und Fokus Navigation.

### <a name="wpf-and-windows-forms"></a>WPF und Windows Forms

Das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement im Windows Community Toolkit dient als Referenzbeispiel für die Verwendung der UWP-Hosting-API in WPF-und Windows Forms-apps. Der Quellcode ist an den folgenden Speicherorten verfügbar:

* Die WPF-Version des Steuer Elements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Die WPF-Version wird von [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)abgeleitet.

* Die Windows Forms-Version des Steuer Elements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Die Windows Forms Version wird von [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)abgeleitet.

## <a name="host-a-standard-uwp-control"></a>Hosten eines UWP-Standard Steuer Elements

In diesem Abschnitt werden Sie durch den Prozess der Verwendung der UWP-XAML-Hosting-API zum Hosten eines UWP-Standard Steuer Elements (d. h. eines Steuer Elements, das von C++ der Windows SDK-oder WinUI-Bibliothek bereitgestellt wird) in einer neuen Win32 Der Code basiert auf dem [einfachen XAML-Insel Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App), und in diesem Abschnitt werden einige der wichtigsten Teile des Codes erläutert. Wenn Sie über ein vorhandenes C++ Win32-App-Projekt verfügen, können Sie diese Schritte und Codebeispiele für Ihr Projekt anpassen.

### <a name="configure-the-project"></a>Konfigurieren des Projekts

1. Erstellen Sie in Visual Studio 2019 mit Windows 10, Version 1903 SDK (Version 10.0.18362) oder einer neueren Version, ein neues **Windows-Desktop Anwendungs** Projekt. Dieses Projekt ist unter den **C++** Projekt filtern, **Windows**und **Desktop** verfügbar.

2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappenknoten, klicken Sie auf Projekt Mappe **neu**zuweisen, wählen Sie die Version **10.0.18362.0** oder höher aus, und klicken Sie dann auf **OK**.

3. Installieren Sie das nuget-Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) :

    1. Klicken Sie mit der rechten Maustaste auf das Projekt in **Projektmappen-Explorer** und wählen Sie **nuget-Pakete verwalten**.
    2. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) , und installieren Sie die neueste Version dieses Pakets.

4. Installieren Sie das nuget-Paket " [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) ":

    1. Vergewissern Sie sich, dass im Fenster **nuget-Paket-Manager** die Option **Vorabversion einschließen** ausgewählt ist.
    2. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) , und installieren Sie Version v 6.0.0 (oder höher) dieses Pakets.

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Verwenden der XAML-Hosting-API zum Hosten eines UWP-Steuer Elements

Der grundlegende Prozess der Verwendung der XAML-Hosting-API zum Hosten eines UWP-Steuer Elements befolgt die folgenden allgemeinen Schritte:

1. Initialisieren Sie das UWP-XAML-Framework für den aktuellen Thread, bevor Ihre APP eines der [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) -Objekte erstellt, die Sie hosten wird. Es gibt mehrere Möglichkeiten, dies zu tun, je nachdem, wann das [desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) -Objekt erstellt werden soll, das die Steuerelemente hostet.

    * Wenn Ihre Anwendung das **desktopwindowxamlsource** -Objekt erstellt, bevor Sie eines der **Windows. UI. XAML. UIElement** -Objekte erstellt, das gehostet wird, wird dieses Framework für Sie initialisiert, wenn Sie das **desktopwindowxamlsource** -Objekt instanziieren. In diesem Szenario müssen Sie keinen eigenen Code hinzufügen, um das Framework zu initialisieren.

    * Wenn die Anwendung jedoch die **Windows. UI erstellt. XAML. UIElement** -Objekte bevor das **desktopwindowxamlsource** -Objekt erstellt wird, das Sie hostet, muss Ihre Anwendung die statische [windowsxamlmanager. initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) -Methode aufrufen, um das UWP-XAML-Framework explizit zu initialisieren, bevor die **Windows. UI. XAML** Diese Methode sollte in der Regel von Ihrer Anwendung aufgerufen werden, wenn das übergeordnete UI-Element, das **desktopwindowxamlsource** hostet, instanziiert wird.

    > [!NOTE]
    > Diese Methode gibt ein [windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) -Objekt zurück, das einen Verweis auf das UWP-XAML-Framework enthält. Sie können in einem bestimmten Thread beliebig viele **windowsxamlmanager** -Objekte erstellen. Da jedes Objekt jedoch einen Verweis auf das UWP-XAML-Framework enthält, sollten Sie die Objekte verwerfen, um sicherzustellen, dass XAML-Ressourcen schließlich freigegeben werden.

2. Erstellen Sie ein [desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) -Objekt, und fügen Sie es an ein übergeordnetes Benutzeroberflächen Element in der Anwendung an, das einem Fenster Handle zugeordnet ist.

    Zu diesem Zweck müssen Sie die folgenden Schritte ausführen:

    1. Erstellen Sie ein **desktopwindowxamlsource** -Objekt, und wandeln Sie es in die Schnittstelle **idesktopwindowxamlsourcenative** oder **IDesktopWindowXamlSourceNative2** com um.
        > [!NOTE]
        > Diese Schnittstellen werden in der **Windows. UI. XAML. Hosting. desktopwindowxamlsource. h** -Header Datei im Windows SDK deklariert. Standardmäßig befindet sich diese Datei unter% Program Files (x86)% \ Windows kits\10\include\\< Buildnummer\>\e.

    2. Ruft die **attachtowindow** -Methode der **idesktopwindowxamlsourcenative** -Schnittstelle oder der **IDesktopWindowXamlSourceNative2** -Schnittstelle auf und übergibt das Fenster Handle des übergeordneten UI-Elements in der Anwendung.

    3. Legen Sie die anfängliche Größe des internen untergeordneten Fensters fest, das in der **desktopwindowxamlsource**enthalten ist. Standardmäßig ist dieses interne untergeordnete Fenster auf eine Breite und Höhe von 0 festgelegt. Wenn Sie die Größe des Fensters nicht festlegen, werden alle UWP-Steuerelemente, die Sie der **desktopwindowxamlsource** hinzufügen, nicht angezeigt. Um auf das interne untergeordnete Fenster in der **desktopwindowxamlsource**zuzugreifen, verwenden Sie die **WindowHandle** -Eigenschaft der **idesktopwindowxamlsourcenative** -Schnittstelle oder der **IDesktopWindowXamlSourceNative2** -Schnittstelle.

3. Weisen Sie schließlich das **Windows. UI. XAML. UIElement** , das Sie hosten möchten, der [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) -Eigenschaft des **desktopwindowxamlsource** -Objekts zu.

Die folgenden Schritte und Codebeispiele veranschaulichen, wie der oben beschriebene Prozess implementiert wird:

1. Öffnen Sie im Ordner **Quelldateien** des Projekts die Standarddatei **windowsproject. cpp** . Löschen Sie den gesamten Inhalt der Datei, und fügen Sie die folgenden `include`-und `using`-Anweisungen hinzu. Zusätzlich zu den Standard C++ -und UWP-Headern und-Namespaces enthalten diese Anweisungen mehrere Elemente, die für XAML-Inseln spezifisch sind.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. Kopieren Sie den folgenden Code nach dem vorherigen Abschnitt. Mit diesem Code wird die **WinMain** -Funktion für die APP definiert. Diese Funktion initialisiert ein einfaches Fenster und verwendet die XAML-Hosting-API, um ein einfaches UWP- **TextBlock** -Steuerelement im-Fenster zu hosten.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. Kopieren Sie den folgenden Code nach dem vorherigen Abschnitt. Dieser Code definiert die [Fenster Prozedur](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure) für das Fenster.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. Speichern Sie die Codedatei, erstellen Sie die APP, und führen Sie Sie aus. Vergewissern Sie sich, dass das UWP- **TextBlock** -Steuerelement im App-Fenster angezeigt wird.
    > [!NOTE]
    > Möglicherweise werden mehrere Buildwarnungen angezeigt, einschließlich `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` und `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Diese Warnungen sind bekannte Probleme mit den aktuellen Tools und nuget-Paketen und können ignoriert werden.

Ausführliche Beispiele für diese Aufgaben finden Sie in den folgenden Code Dateien:

* **C++Win32**
  * Weitere Informationen finden Sie in der Datei " [hellowindowsdesktop. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) ".
  * Weitere Informationen finden Sie in der [xamlbridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) -Datei.
* **WPF:** Weitere Informationen finden Sie in den Dateien [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) im Windows Community Toolkit.  

* **Windows Forms:** Weitere Informationen finden Sie in den Dateien [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) im Windows Community Toolkit.

## <a name="host-a-custom-uwp-control"></a>Hosten eines benutzerdefinierten UWP-Steuer Elements

Der Prozess zum Hosting eines benutzerdefinierten UWP-Steuer Elements (entweder ein von Ihnen definiertes Steuerelement oder ein Steuerelement, das von C++ einem Drittanbieter bereitgestellt wird) in einer Win32-App beginnt mit den gleichen Schritten wie das [Hosting eines Standard Steuer](#host-a-standard-uwp-control)Elements. Zum Hosting eines benutzerdefinierten UWP-Steuer Elements sind jedoch zusätzliche Projekte und Schritte erforderlich.

Um ein benutzerdefiniertes UWP-Steuerelement zu hosten, benötigen Sie die folgenden Projekte und Komponenten:

* **Das Projekt und den Quellcode für Ihre APP**. Stellen Sie sicher, dass Sie Ihr Projekt so konfiguriert haben, dass die [Voraussetzungen](#prerequisites) für das Hosting von XAML-Inseln

* **Das benutzerdefinierte UWP-Steuer**Element. Sie benötigen den Quellcode für das benutzerdefinierte UWP-Steuerelement, das Sie hosten möchten, damit Sie es mit Ihrer APP kompilieren können. In der Regel wird das benutzerdefinierte Steuerelement in einem UWP-Klassen Bibliotheksprojekt definiert, auf das Sie in C++ derselben Projekt Mappe wie das Win32-Projekt verweisen.

* **Ein UWP-App-Projekt, das ein xamlapplication-Objekt definiert**. Das C++ Win32-Projekt muss über Zugriff auf eine Instanz der [Microsoft. Toolkit. Win32. UI. xamlhost. xamlapplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) -Klasse verfügen, die vom Windows Community Toolkit bereitgestellt wird. Dieser Typ fungiert als Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen in Assemblys im aktuellen Verzeichnis der Anwendung.

  Die empfohlene Vorgehensweise besteht darin, ein **leeres App-Projekt (Universal Windows)** zur gleichen Projekt Mappe wie das C++ Win32-Projekt hinzuzufügen, die Standard `App` Klasse in diesem Projekt zu überarbeiten, um von `XamlApplication`abzuleiten, und dann eine Instanz dieses Objekts im Code für den C++ Einstiegspunkt für ihre Win32-APP zu erstellen.

  > [!NOTE]
  > Die Projekt Mappe kann nur ein Projekt enthalten, das ein `XamlApplication` Objekt definiert. Alle benutzerdefinierten UWP-Steuerelemente in Ihrer APP verwenden dasselbe `XamlApplication` Objekt. Das Projekt, das das `XamlApplication` Objekt definiert, muss Verweise auf alle anderen UWP-Bibliotheken und-Projekte enthalten, die in der XAML-Insel als Host-UWP-Steuerelemente verwendet werden.

Führen Sie die folgenden allgemeinen Schritte aus, um C++ ein benutzerdefiniertes UWP-Steuerelement in einer Win32-APP zu

1. Fügen Sie in der Projekt Mappe C++ , die das Win32-Desktop-App-Projekt enthält, ein **leeres App-Projekt (Universal Windows)** hinzu, und definieren Sie eine `XamlApplication` Klasse, indem Sie die ausführlichen Anweisungen in [diesem Abschnitt](host-custom-control-with-xaml-islands.md#define-a-xamlapplication-class-in-a-uwp-app-project) in der zugehörigen exemplarischen Vorgehensweise zu WPF befolgen. 

2. Fügen Sie in der gleichen Projekt Mappe das Projekt hinzu, das den Quellcode für das benutzerdefinierte UWP-XAML-Steuerelement enthält (in der Regel ein UWP-Klassen Bibliotheksprojekt), und erstellen Sie das Projekt.

3. Fügen Sie im UWP-App-Projekt einen Verweis auf das UWP-Klassen Bibliotheksprojekt hinzu.

4. In Ihrem C++ Win32-Projekt:

  * Fügen Sie einen Verweis auf das UWP-App-Projekt und das UWP-Klassen Bibliotheksprojekt in der Projekt Mappe hinzu.
  * Erstellen Sie in der `WinMain`-Funktion oder einem anderen Code für den Einstiegspunkt eine Instanz der `XamlApplication`-Klasse, die Sie zuvor im UWP-App-Projekt definiert haben. Beispielsweise finden Sie [diese Codezeile](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Desktop_Win32App/DesktopWin32App/DesktopWin32App.cpp#L46) aus dem C++ Win32-Beispiel in den [Beispielen für die XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples).

5. Befolgen Sie den im Abschnitt [Verwenden der XAML-Hosting-API zum Hosten eines UWP-Steuer](#use-the-xaml-hosting-api-to-host-a-uwp-control) Elements beschriebenen Prozess zum Hosten des benutzerdefinierten Steuer Elements in einer XAML-Insel in Ihrer APP. Weisen Sie eine Instanz des benutzerdefinierten Steuer Elements zu, um die [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) -Eigenschaft des **desktopwindowxamlsource** -Objekts im Code zu hosten.

Ein vollständiges Beispiel für eine C++ Win32-Anwendung finden Sie im [XAML C++ -Insel-Win32-Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Desktop_Win32App).

## <a name="handle-keyboard-layout-and-dpi"></a>Behandeln von Tastatur, Layout und dpi

Die folgenden Abschnitte enthalten Anleitungen und Links zu Codebeispielen zum Behandeln von Tastatur-und Layoutszenarien.

* [Tastatureingabe](#keyboard-input)
* [Tastaturfokus Navigation](#keyboard-focus-navigation)
* [Behandeln von Layoutänderungen](#handle-layout-changes)
* [Behandeln von dpi-Änderungen](#handle-dpi-changes)

### <a name="keyboard-input"></a>Tastatureingabe

Damit Tastatureingaben für jede XAML-Insel ordnungsgemäß behandelt werden, muss die Anwendung alle Windows-Meldungen an das UWP-XAML-Framework übergeben, damit bestimmte Nachrichten ordnungsgemäß verarbeitet werden können. Wandeln Sie das **desktopwindowxamlsource** -Objekt für jede XAML-Insel in eine **IDesktopWindowXamlSourceNative2** -com-Schnittstelle ein, um dies zu erreichen. Rufen Sie dann die **pretranslatemess Age** -Methode dieser Schnittstelle auf, und übergeben Sie die aktuelle Nachricht.

  * Win32:: die APP kann **pretranslatemess** direkt in der Hauptnachrichten Schleife aufzurufen. **C++** Ein Beispiel finden Sie in der Datei " [xamlbridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) ".

  * **WPF:** Die APP kann **pretranslatemess** aus dem Ereignishandler für das [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) -Ereignis aufrufen. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) im Windows Community Toolkit.

  * **Windows Forms:** Die APP kann **pretranslatemess** aus einer außer Kraft setzung für die [Control. PreProcessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) -Methode abrufen. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) im Windows Community Toolkit.

### <a name="keyboard-focus-navigation"></a>Tastaturfokus Navigation

Wenn der Benutzer die Benutzeroberflächen Elemente in der Anwendung über die Tastatur navigiert (z. b. durch Drücken der **Tab** -Taste oder der Richtung/Pfeiltaste), müssen Sie den Fokus Programm gesteuert auf das **desktopwindowxamlsource** -Objekt verschieben. Wenn die Tastaturnavigation des Benutzers die **desktopwindowxamlsource**erreicht, verschieben Sie den Fokus auf das erste [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) -Objekt in der Navigations Reihenfolge für die Benutzeroberfläche, verschieben Sie den Fokus auf die folgenden **Windows. UI. XAML. UIElement** -Objekte, während der Benutzer die Elemente durchläuft, und verschieben Sie dann den Fokus aus **desktopwindowxamlsource** und in das übergeordnete UI  

Die UWP-XAML-Hosting-API bietet verschiedene Typen und Member, die Sie beim Ausführen dieser Aufgaben unterstützen.

* Wenn die Tastaturnavigation in Ihre **desktopwindowxamlsource**gelangt, wird das Ereignis " [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) " ausgelöst. Behandeln Sie dieses Ereignis, und verschieben Sie den Fokus Programm gesteuert auf das erste gehostete **Windows. UI. XAML. UIElement** mithilfe der [navigatefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) -Methode.

* Wenn der Benutzer das letzte Fokussier Bare Element in der **desktopwindowxamlsource** verwendet und die **Tab** -Taste oder eine Pfeiltaste drückt, wird das Ereignis " [takefoceangeforderten](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) " ausgelöst. Behandeln Sie dieses Ereignis, und verschieben Sie den Fokus Programm gesteuert auf das nächste Fokussier Bare Element in der Host Anwendung. Beispielsweise können Sie in einer WPF-Anwendung, in der **desktopwindowxamlsource** in einem [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)gehostet wird, die Methode ["wvefocus"](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) verwenden, um den Fokus auf das nächste Fokussier Bare Element in der Host Anwendung z übertragen.

Beispiele für die Vorgehensweise im Kontext einer funktionierenden Beispielanwendung finden Sie in den folgenden Code Dateien:

  * /Win32: Weitere Informationen finden Sie in der Datei " [xamlbridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) ".  **C++**

  * **WPF:** Weitere Informationen finden Sie in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) im Windows Community Toolkit.  

  * **Windows Forms:** Weitere Informationen finden Sie in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) im Windows Community Toolkit.

### <a name="handle-layout-changes"></a>Behandeln von Layoutänderungen

Wenn der Benutzer die Größe des übergeordneten Elements der Benutzeroberfläche ändert, müssen Sie alle notwendigen Layoutänderungen verarbeiten, um sicherzustellen, dass Ihre UWP-Steuerelemente erwartungsgemäß angezeigt werden. Hier sind einige wichtige Szenarien, die berücksichtigt werden müssen.

* Wenn die C++ Anwendung in einer Win32-Anwendung die WM_SIZE Nachricht verarbeitet, kann Sie die gehostete XAML-Insel mithilfe der [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) -Funktion neu positionieren. Ein Beispiel finden Sie in der Codedatei [SampleApp. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) .

* Wenn das übergeordnete Element der Benutzeroberfläche die Größe des rechteckigen Bereichs abrufen muss, der für das **Windows. UI. XAML. UIElement** erforderlich ist, das Sie auf dem **desktopwindowxamlsource**-Element gehostet haben, müssen Sie die [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) -Methode von **Windows. UI. XAML. UIElement**aufzurufen. Zum Beispiel:

    * In einer WPF-Anwendung können Sie dies über die " [accessreoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) "-Methode von [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) tun, der " **desktopwindowxamlsource**" hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

    * In einer Windows Forms Anwendung können Sie dies über die [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) -Methode des [Steuer](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Elements tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

* Wenn sich die Größe des übergeordneten Elements der Benutzeroberfläche ändert, können Sie die [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)-Methode des Stammelements **Windows.UI.Xaml.UIElement** aufrufen, das Sie auf der **DesktopWindowXamlSource** hosten. Zum Beispiel:

    * In einer WPF-Anwendung können Sie dies über die [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) -Methode des [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) -Objekts tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

    * In einer Windows Forms Anwendung können Sie dies über den Handler für das [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) -Ereignis des [Steuer](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Elements tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

### <a name="handle-dpi-changes"></a>Behandeln von dpi-Änderungen

Das UWP-XAML-Framework verarbeitet dpi-Änderungen für gehostete UWP-Steuerelemente automatisch (z. b. wenn der Benutzer das Fenster zwischen Monitoren mit unterschiedlichen Bildschirm dpi zieht). Um die optimale Leistung zu erzielen, empfiehlt es sich, Ihre Windows Forms-, C++ WPF-oder Win32-Anwendung so zu konfigurieren, dass Sie pro Monitor-dpi-Wert ist.

Fügen Sie Ihrem Projekt ein paralleles Assemblymanifest hinzu, und [legen Sie das](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)  **\<dpiawareness\>** -Element auf **PerMonitorV2**fest, um die Anwendung pro Monitor-dpi-Wert zu konfigurieren. Weitere Informationen zu diesem Wert finden Sie in der Beschreibung für [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="troubleshooting"></a>Fehlerbehebung

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Fehler beim Verwenden der UWP-XAML-Hosting-API in einer UWP-App

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "desktopwindowxamlsource kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden. " oder "windowsxamlmanager kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden. " | Dieser Fehler weist darauf hin, dass Sie versuchen, die UWP-XAML-Hosting-API (insbesondere die Typen " [desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) " oder " [windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ") in einer UWP-APP zu verwenden. Die UWP-XAML-Hosting-API ist nur für die Verwendung in Desktop-Apps ohne UWP (z. b. WPF C++ -, Windows Forms-und Win32-Anwendungen) vorgesehen. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Fehler beim Versuch, die Typen "windowsxamlmanager" oder "desktopwindowxamlsource" zu verwenden.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine Ausnahme mit folgender Meldung: "windowsxamlmanager und desktopwindowxamlsource werden für Apps unterstützt, die auf Windows-Version 10.0.18226.0 und höher abzielen. Überprüfen Sie entweder das Anwendungs Manifest oder das Paket Manifest, und stellen Sie sicher, dass die maxtestedversion-Eigenschaft aktualisiert wird. " | Dieser Fehler zeigt an, dass die Anwendung versucht hat, die Typen **windowsxamlmanager** oder **desktopwindowxamlsource** in der UWP-XAML-Hosting-API zu verwenden, aber das Betriebssystem kann nicht bestimmen, ob die APP für das Ziel Windows 10, Version 1903 oder höher, entwickelt wurde. Die UWP-XAML-Hosting-API wurde erstmals als Vorschauversion in einer früheren Version von Windows 10 eingeführt, wird jedoch nur ab Windows 10, Version 1903, unterstützt.</p></p>Um dieses Problem zu beheben, erstellen Sie entweder ein msix-Paket für die APP, führen Sie es aus dem Paket aus, oder installieren Sie das nuget-Paket [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) in Ihrem Projekt. Weitere Informationen finden Sie in [diesem Abschnitt](#configure-your-project-for-app-deployment). |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Fehler beim Anfügen an ein Fenster in einem anderen Thread.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "die attachdewindow-Methode konnte nicht ausgeführt werden, da das angegebene HWND in einem anderen Thread erstellt wurde". | Dieser Fehler weist darauf hin, dass die Anwendung die **idesktopwindowxamlsourcenative:: attachdewindow** -Methode aufgerufen hat und ihr das HWND eines Fensters, das in einem anderen Thread erstellt wurde, weitergereicht hat. Sie müssen diese Methode als HWND eines Fensters übergeben, das im gleichen Thread wie der Code erstellt wurde, von dem aus Sie die-Methode aufrufen. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Fehler beim Anfügen an ein Fenster in einem anderen Fenster der obersten Ebene.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "die attachdewindow-Methode konnte nicht ausgeführt werden, da das angegebene HWND von einem anderen Fenster der obersten Ebene abgeleitet ist als das HWND, das zuvor an attachtewindow im gleichen Thread weitergegeben wurde." | Dieser Fehler zeigt an, dass die Anwendung die **idesktopwindowxamlsourcenative:: attachdewindow** -Methode aufgerufen und das HWND eines Fensters an das Fenster weitergeleitet hat, das von einem anderen Fenster der obersten Ebene abgeleitet ist als ein Fenster, das Sie in einem vorherigen Aufruf dieser Methode im gleichen Thread angegeben haben.</p></p>Nachdem die Anwendung " **attachdewindow** " für einen bestimmten Thread aufgerufen hat, können alle anderen [desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) -Objekte im gleichen Thread nur an Fenster angefügt werden, die nachfolgende Elemente desselben Fensters der obersten Ebene sind, das beim ersten Aufruf von " **AttachTo Window**" weitergegeben wurde. Wenn alle **desktopwindowxamlsource** -Objekte für einen bestimmten Thread geschlossen sind, kann die nächste **desktopwindowxamlsource** wieder an jedes Fenster angefügt werden.</p></p>Um dieses Problem zu beheben, schließen Sie entweder alle **desktopwindowxamlsource** -Objekte, die an andere Fenster der obersten Ebene in diesem Thread gebunden sind, oder erstellen Sie einen neuen Thread für diese **desktopwindowxamlsource**. |

## <a name="related-topics"></a>Zugehörige Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md)
* [C++Beispiel für Win32-XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
