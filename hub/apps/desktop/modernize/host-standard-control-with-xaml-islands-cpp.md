---
description: In diesem Artikel wird veranschaulicht, wie ein Standardmäßiges UWP- C++ Steuerelement in einer Win32-App mithilfe der XAML-Hosting-API gehostet wird.
title: Hosten eines UWP-Standard Steuer C++ Elements in einer Win32-App mithilfe von XAML-Inseln
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, cpp, Win32, XAML-Inseln, umschließende Steuerelemente, Standard Steuerelemente
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226274"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>Hosten eines UWP-Standard Steuer C++ Elements in einer Win32-App

In diesem Artikel wird veranschaulicht, wie die [UWP-XAML-Hosting-API](using-the-xaml-hosting-api.md) verwendet wird, um ein Standardmäßiges UWP-Steuerelement (d. h. ein C++ Steuerelement, das vom Windows SDK bereitgestellt wird) in einer neuen Win32- Der Code basiert auf dem [einfachen XAML-Insel Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App), und in diesem Abschnitt werden einige der wichtigsten Teile des Codes erläutert. Wenn Sie über ein vorhandenes C++ Win32-App-Projekt verfügen, können Sie diese Schritte und Codebeispiele für Ihr Projekt anpassen.

> [!NOTE]
> Das in diesem Artikel gezeigte Szenario unterstützt das direkte Bearbeiten von XAML-Markup für UWP-Steuerelemente, die in Ihrer APP gehostet werden. Dieses Szenario schränkt Sie ein, um die Darstellung und das Verhalten von gehosteten UWP-Steuerelementen über Code zu ändern. Anweisungen zum direkten Bearbeiten von XAML-Markup beim Hosten von UWP-Steuerelementen finden Sie unter [Hosten eines benutzerdefinierten C++ UWP-Steuer Elements in einer Win32-App](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Erstellen eines Desktop Anwendungs Projekts

1. Erstellen Sie in Visual Studio 2019 mit Windows 10, Version 1903 SDK (Version 10.0.18362) oder einer neueren Version, ein neues **Windows-Desktop Anwendungs** Projekt, und nennen Sie es **MyDesktopWin32App**. Dieser Projekttyp ist unter **C++** den Projekt filtern, **Windows**und **Desktop** verfügbar.

2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappenknoten, klicken Sie auf Projekt Mappe **neu**zuweisen, wählen Sie die Version **10.0.18362.0** oder höher aus, und klicken Sie dann auf **OK**.

3. Installieren Sie das nuget-Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) , um die Unterstützung für [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) im Projekt zu aktivieren:

    1. Klicken Sie mit der rechten Maustaste auf das Projekt in **Projektmappen-Explorer** und wählen Sie **nuget-Pakete verwalten**.
    2. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) , und installieren Sie die neueste Version dieses Pakets.

    > [!NOTE]
    > Bei neuen Projekten können Sie alternativ die [ C++/WinRT Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installieren und eine der C++/WinRT-Projektvorlagen verwenden, die in dieser Erweiterung enthalten sind. Weitere Informationen finden Sie in [diesem Artikel](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. Installieren Sie das nuget-Paket " [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) ":

    1. Vergewissern Sie sich, dass im Fenster **nuget-Paket-Manager** die Option **Vorabversion einschließen** ausgewählt ist.
    2. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket **Microsoft. Toolkit. Win32. UI. SDK** , und installieren Sie Version v 6.0.0 (oder höher) dieses Pakets. Dieses Paket enthält mehrere Build-und Lauf Zeit Objekte, die es ermöglichen, dass XAML-Inseln in der App funktionieren.

5. Legen Sie den `maxVersionTested` Wert im [Anwendungs Manifest](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) fest, um anzugeben, dass die Anwendung mit Windows 10, Version 1903 oder höher, kompatibel ist.

    1. Wenn Sie nicht bereits über ein Anwendungs Manifest in Ihrem Projekt verfügen, fügen Sie dem Projekt eine neue XML-Datei hinzu, und nennen Sie Sie " **app. Manifest**".
    2. Fügen Sie im Anwendungs Manifest das **Compatibility** -Element und die untergeordneten Elemente ein, die im folgenden Beispiel gezeigt werden. Ersetzen Sie das **ID** -Attribut des **maxversiongetesteten** Elements durch die Versionsnummer von Windows 10, die Sie als Ziel festgelegt haben (Dies muss Windows 10, Version 1903 oder eine höhere Version sein).

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

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Verwenden der XAML-Hosting-API zum Hosten eines UWP-Steuer Elements

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

1. Öffnen Sie im Ordner **Quelldateien** des Projekts die Standarddatei **MyDesktopWin32App. cpp** . Löschen Sie den gesamten Inhalt der Datei, und fügen Sie die folgenden `include`-und `using`-Anweisungen hinzu. Zusätzlich zu den Standard C++ -und UWP-Headern und-Namespaces enthalten diese Anweisungen mehrere Elemente, die für XAML-Inseln spezifisch sind.

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

## <a name="package-the-app"></a>Verpacken der APP

Optional können Sie die app in einem [msix-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung verpacken. Msix ist die moderne App-Paket Technologie für Windows und basiert auf einer Kombination aus MSI-, AppX-, App-V-und ClickOnce-Installationstechnologien.

Die folgenden Anweisungen zeigen, wie Sie alle Komponenten in der Projekt Mappe in einem msix-Paket Verpacken, indem Sie das [Windows-Anwendungs](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) Paket in Visual Studio 2019 verwenden. Diese Schritte sind nur erforderlich, wenn Sie die app in einem msix-Paket verpacken möchten.

> [!NOTE]
> Wenn Sie die Anwendung nicht in einem [msix-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung packen, muss auf Computern, auf denen Ihre APP ausgeführt wird, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

1. Fügen Sie der Projekt Mappe ein neues [Windows-Anwendungspaket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wählen Sie beim Erstellen des Projekts die Option **Windows 10, Version 1903 (10,0; Build 18362)** für die **Zielversion** und die **Mindestversion**.

2. Klicken Sie im Paket Projekt mit der rechten Maustaste auf den Knoten **Anwendungen** , und wählen Sie **Verweis hinzufügen**aus. Wählen Sie in der Liste der Projekte das C++/Win32-Desktop Anwendungsprojekt in der Projekt Mappe aus, und klicken Sie auf **OK**.

3. Erstellen Sie das Verpackungsprojekt, und führen Sie es aus. Vergewissern Sie sich, dass die app ausgeführt wird und die UWP-Steuerelemente erwartungsgemäß anzeigt.

## <a name="next-steps"></a>Nächste Schritte

Die Codebeispiele in diesem Artikel erleichtern Ihnen den Einstieg in das grundlegende Szenario für das Hosting eines standardmäßigen UWP-Steuer Elements in einer C++ Win32-app. In den folgenden Abschnitten werden zusätzliche Szenarien vorgestellt, die möglicherweise von Ihrer Anwendung unterstützt werden müssen.

### <a name="host-a-custom-uwp-control"></a>Hosten eines benutzerdefinierten UWP-Steuer Elements

In vielen Szenarien müssen Sie möglicherweise ein benutzerdefiniertes UWP-XAML-Steuerelement hosten, das mehrere einzelne Steuerelemente enthält, die zusammenarbeiten. Der Prozess zum Hosting eines benutzerdefinierten UWP-Steuer Elements (entweder ein von Ihnen definiertes Steuerelement oder ein Steuerelement, das von C++ einem Drittanbieter bereitgestellt wird) in einer Win32-APP ist komplexer als das Hosting eines Standard Steuer Elements und erfordert zusätzlichen Code.

Eine umfassende Exemplarische Vorgehensweise finden [Sie unter Hosten eines benutzerdefinierten UWP-Steuer Elements in einer C++ Win32-App mithilfe der XAML-Hosting-API](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Erweiterte Szenarios

Viele Desktop Anwendungen, die XAML-Inseln hosten, müssen zusätzliche Szenarios verarbeiten, um eine reibungslose Benutzer Leistung zu gewährleisten. Beispielsweise müssen Desktop Anwendungen möglicherweise Tastatureingaben in XAML-Inseln verarbeiten, die Navigation zwischen XAML-Inseln und anderen Benutzeroberflächen Elementen und Layoutänderungen steuern.

Weitere Informationen zur Behandlung dieser Szenarien und Zeiger auf Verwandte Codebeispiele finden Sie unter [Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Verwandte Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md)
* [Verwenden der UWP-XAML-Hosting- C++ API in einer Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines benutzerdefinierten UWP- C++ Steuer Elements in einer Win32-App](host-custom-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps](advanced-scenarios-xaml-islands-cpp.md)
* [Codebeispiele für XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples)
