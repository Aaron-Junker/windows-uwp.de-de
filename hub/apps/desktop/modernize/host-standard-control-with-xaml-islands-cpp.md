---
description: Dieser Artikel veranschaulicht, wie ein UWP-Standardsteuerelement mithilfe der XAML-Hosting-API in einer C++-Win32-App gehostet wird.
title: Hosten eines UWP-Standardsteuerelements in einer C++-Win32-App unter Verwendung von XAML Islands
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, XAML Islands, umschlossene Steuerelemente, Standardsteuerelemente
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0842046419402bbfacc24331d0521efa9510153a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174194"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>Hosten eines UWP-Standardsteuerelements in einer C++-Win32-App

Dieser Artikel veranschaulicht, wie mithilfe der [XAML-Hosting-API](using-the-xaml-hosting-api.md) ein UWP-Standardsteuerelement (d. h., ein vom Windows SDK bereitgestelltes Steuerelement) in einer neuen C++-Win32-App verwendet wird. Der Code basiert auf dem [einfachen XAML Islands-Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App), und in diesem Abschnitt werden einige der wichtigen Teile des Codes besprochen. Wenn du über ein vorhandenes C++-Win32-Projekt verfügst, kannst du diese Schritte und Codebeispiele für dein Projekt anpassen.

> [!NOTE]
> Das in diesem Artikel veranschaulichte Szenario bietet keine direkte Unterstützung für die Bearbeitung von XAML-Markup für UWP-Steuerelemente, die in deiner App gehostet werden. In diesem Szenario bist du darauf beschränkt, das Aussehen und Verhalten der gehosteten UWP-Steuerelemente über den Code zu ändern. Informationen dazu, wie XAML-Markup beim Hosten von UWP-Steuerelementen direkt bearbeitet werden kann, findest du unter [Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++ Win32-App](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Erstellen eines Desktopanwendungsprojekts

1. Erstelle in Visual Studio 2019 mit dem SDK für Windows 10, Version 1903 (Version 10.0.18362) oder höher ein neues Projekt vom Typ **Windows-Desktopanwendung**, und gib ihm den Namen **MyDesktopWin32App**. Dieser Projekttyp steht unter den Projektfiltern **C++** , **Windows** und **Desktop** zur Verfügung.

2. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, klicke auf **Projektmappe neu zuweisen**, wähle **10.0.18362.0** oder eine höhere SDK-Version aus, und klicke dann auf **OK**.

3. Installiere das NuGet-Paket [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), um die Unterstützung für [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) in deinem Projekt zu aktivieren:

    1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf dein Projekt, und wähle **NuGet-Pakete verwalten** aus.
    2. Wähle die Registerkarte **Durchsuchen** aus, suche nach dem Paket [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), und installiere die neueste Version dieses Pakets.

    > [!NOTE]
    > Für neue Projekte kannst du alternativ die [C++/WinRT-Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installieren und eine der in dieser Erweiterung enthaltenen C++/WinRT-Projektvorlagen verwenden. Weitere Informationen finden Sie in [diesem Artikel](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. Installiere das NuGet-Paket [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK):

    1. Stelle sicher, dass im Fenster **NuGet-Paket-Manager** die Einstellung **Vorabversion einbeziehen** ausgewählt ist.
    2. Wähle die Registerkarte **Durchsuchen** aus, suche nach dem Paket **Microsoft.Toolkit.Win32.UI.SDK**, und installiere Version v6.0.0 dieses Pakets (oder eine höhere Version). Dieses Paket stellt verschiedene Ressourcen für Kompilier- und Laufzeit bereit, um XAML Islands in deiner App zu aktivieren.

5. Lege den `maxVersionTested`-Wert in deinem [Anwendungsmanifest](/windows/desktop/SbsCs/application-manifests) fest, um anzugeben, dass deine Anwendung mit Windows 10, Version 1903 oder höher kompatibel ist.

    1. Falls noch kein Anwendungsmanifest in deinem Projekt enthalten ist, füge deinem Projekt eine neue XML-Datei hinzu, und gibt ihr den Namen **app.manifest**.
    2. Füge das **compatibility**-Element und die untergeordneten Elemente wie im folgenden Beispiel gezeigt in dein Anwendungsmanifest ein. Ersetze das **Id**-Attribut des **maxVersionTested**-Elements durch die Versionsnummer von Windows 10, die du als Zielplattform verwenden möchtest (hierbei muss es sich um Windows 10, Version 1903 oder um eine höhere Version handeln).

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

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Verwenden der XAML-Hosting-API zum Hosten eines UWP-Steuerelements

Das grundlegende Verfahren bei Verwendung der XAML-Hosting-API zum Hosten eines UWP-Steuerelements umfasst diese allgemeinen Schritte:

1. Initialisieren des UWP-XAML-Frameworks für den aktuellen Thread, bevor deine App eines der [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement)-Objekte erstellt, die gehostet werden sollen. Je nachdem, wann du das [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)-Objekt zum Hosten der Steuerelemente erstellst, gibt es hierfür mehrere Möglichkeiten.

    * Wenn deine Anwendung das **DesktopWindowXamlSource**-Objekt erstellt, bevor eines der zu hostenden **Windows.UI.Xaml.UIElement**-Objekte erstellt wird, wird dieses Framework beim Instanziieren des **DesktopWindowXamlSource**-Objekts für dich initialisiert. In diesem Szenario ist es nicht erforderlich, eigenen Code zum Initialisieren des Frameworks hinzuzufügen.

    * Wenn deine Anwendung die **Windows.UI.Xaml.UIElement**-Objekte jedoch erstellt, bevor das **DesktopWindowXamlSource**-Objekt zu deren Hosting erstellt wird, muss deine Anwendung vor dem Instanziieren der **Windows.UI.Xaml.UIElement**-Objekte die statische [WindowsXamlManager.InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)-Methode aufrufen, um das UWP-XAML-Framework explizit zu initialisieren. Deine Anwendung sollte diese Methode in der Regel aufrufen, wenn das übergeordnete Benutzeroberflächenelement instanziiert wird, das **DesktopWindowXamlSource** hostet.

    > [!NOTE]
    > Diese Methode gibt ein [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)-Objekt zurück, das einen Verweise auf das UWP-XAML-Framework enthält. Du kannst für einen bestimmten Thread beliebig viele **WindowsXamlManager**-Objekte erstellen. Da jedoch jedes Objekt einen Verweis auf das UWP-XAML-Framework umfasst, solltest du diese Objekte verwerfen, um sicherzustellen, dass die XAML-Ressourcen schließlich freigegeben werden.

2. Erstelle ein [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)-Objekt, und füge es an ein übergeordnetes Benutzeroberflächenelement in deiner Anwendung an, das einem Fensterhandle zugeordnet ist.

    Führe hierzu die folgenden Schritte aus:

    1. Erstelle ein **DesktopWindowXamlSource**-Objekt, und wandle es in die **IDesktopWindowXamlSourceNative**- oder **IDesktopWindowXamlSourceNative2**-COM-Schnittstelle um.
        > [!NOTE]
        > Diese Schnittstellen sind in der Headerdatei **windows.ui.xaml.hosting.desktopwindowxamlsource.h** im Windows SDK deklariert. Standardmäßig befindet sich diese Datei in „%programfiles(x86)%\Windows Kits\10\Include\\<Buildnummer\>\um“.

    2. Rufe die **AttachToWindow**-Methode der Schnittstelle **IDesktopWindowXamlSourceNative** oder **IDesktopWindowXamlSourceNative2** auf, und übergebe das Fensterhandle des übergeordneten Benutzeroberflächenelements in deiner Anwendung.

    3. Lege die anfängliche Größe des internen untergeordneten Fensters fest, das in **DesktopWindowXamlSource** enthalten ist. Standardmäßig ist dieses interne untergeordnete Fenster auf eine Breite und Höhe von 0 festgelegt. Wenn du die Größe des Fensters nicht festlegst, ist keines der UWP-Steuerelemente sichtbar, die du **DesktopWindowXamlSource** hinzufügst. Um in **DesktopWindowXamlSource** auf das interne untergeordnete Fenster zuzugreifen, verwendest du die **WindowHandle**-Eigenschaft der Schnittstelle **IDesktopWindowXamlSourceNative** oder **IDesktopWindowXamlSourceNative2**.

3. Schließlich weist du das zu hostende **Windows.UI.Xaml.UIElement** der [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)-Eigenschaft deines **DesktopWindowXamlSource**-Objekts zu.

Die folgenden Schritte und Codebeispiele veranschaulichen, wie der oben genannte Prozess implementiert wird:

1. Öffne im Ordner **Quelldateien** des Projekts die Standarddatei **MyDesktopWin32App.cpp**. Lösche den gesamten Inhalt der Datei, und füge die folgenden `include`- und `using`-Anweisungen ein. Zusätzlich zu standardmäßigen C++- und UWP-Headern und Namespaces umfassen diese Anweisungen spezifische XAML Islands-Elemente.

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

3. Kopiere den folgenden Code nach dem vorherigen Abschnitt. Dieser Code definiert die **WinMain**-Funktion für die App. Sie initialisiert ein Basisfenster und verwendet die XAML-Hosting-API, um ein einfaches **TextBlock**-UWP-Steuerelement im Fenster zu hosten.

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

4. Kopiere den folgenden Code nach dem vorherigen Abschnitt. Dieser Code definiert die [Fensterprozedur](/windows/win32/learnwin32/writing-the-window-procedure) für das Fenster.

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

5. Speichere die Codedatei, kompiliere die App, und führe sie aus. Vergewissere dich, dass das **TextBlock**-UWP-Steuerelement im App-Fenster angezeigt wird.
    > [!NOTE]
    > Möglicherweise werden verschiedene Kompilierungswarnungen angezeigt, z. B. `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` und `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Diese Warnungen sind bekannte Probleme mit den aktuellen Tools und NuGet-Paketen und können ignoriert werden.

Vollständige Beispiele zum Veranschaulichen dieser Aufgaben findest du in den folgenden Codedateien:

* **C++ Win32:**
  * Siehe Datei [HelloWindowsDesktop.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp).
  * Siehe Datei [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp).
* **WPF:** Siehe Dateien [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) im Windows-Community-Toolkit.  
* **Windows Forms:** Siehe Dateien [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) im Windows-Community-Toolkit.

## <a name="package-the-app"></a>Packen der App

Du kannst die App optional in einem [MSIX-Paket](/windows/msix) für die Bereitstellung packen. MSIX ist eine moderne App-Pakettechnologie für Windows, die auf einer Kombination aus MSI-, APPX-, App-V- und ClickOnce-Installationstechnologien basiert.

Die folgenden Anweisungen veranschaulichen, wie du alle Komponenten in der Projektmappe in ein MSIX-Paket einschließen kannst, indem du die Option [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) in Visual Studio 2019 verwendest. Diese Schritte sind nur erforderlich, wenn du die App in einem MSIX-Paket packen möchtest.

> [!NOTE]
> Wenn du deine Anwendung nicht in einem [MSIX-Paket](/windows/msix) für die Bereitstellung packst, muss auf Computern zur Ausführung deiner App die [Visual C++-Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

1. Fügen Sie Ihrer Projektmappe ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wählen Sie beim Erstellen des Projekts **Windows 10, Version 1903 (10.0; Build 18362)** sowohl für **Zielversion** als auch für **Mindestversion** aus.

2. Klicke im Paketprojekt mit der rechten Maustaste auf den Knoten **Anwendungen**, und wähle **Verweis hinzufügen** aus. Wähle in der Liste der Projekte das C++/Win32-Desktopanwendungsprojekt in deiner Projektmappe aus, und klicke auf **OK**.

3. Kompiliere das Paketerstellungsprojekt, und führe es aus. Vergewissere dich, dass die App ausgeführt und die UWP-Steuerelemente wie erwartet anzeigt.

## <a name="next-steps"></a>Nächste Schritte

In den Codebeispielen in diesem Artikel wurde ein Basisszenario zum Hosten eines UWP-Standardsteuerelements in einer C++-Win32-Anwendung vorgestellt. In den folgenden Abschnitten werden weitere Szenarien vorgestellt, die deine Anwendung möglicherweise unterstützen muss.

### <a name="host-a-custom-uwp-control"></a>Hosten eines benutzerdefinierten UWP-Steuerelements

Für viele Szenarien musst du möglicherweise ein benutzerdefiniertes UWP-XAML-Steuerelement hosten, das mehrere einzelne Steuerelemente enthält, die zusammenarbeiten. Das Verfahren zum Hosten eines benutzerdefinierten UWP-Steuerelements (entweder ein selbst definiertes Steuerelement oder ein von einem Drittanbieter bereitgestelltes Steuerelement) in einer C++ Win32-App ist komplexer als das Hosten eines Standardsteuerelements und erfordert zusätzlichen Code.

Eine vollständige exemplarische Vorgehensweise findest du unter [Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App unter Verwendung der XAML-Hosting-API](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Erweiterte Szenarios

Viele Desktopanwendungen, die XAML Islands hosten, müssen zusätzliche Szenarien verarbeiten, um ein reibungsloses Benutzererlebnis zu gewährleisten. Beispielsweise müssen Desktopanwendungen möglicherweise Tastatureingaben in XAML Islands verarbeiten, den Navigationsfokus zwischen XAML Islands und anderen Benutzeroberflächenelementen verlagern und Layoutänderungen unterstützen.

Weitere Informationen zum Handhaben dieser Szenarien und Verweise auf entsprechende Codebeispiele findest du unter [Erweiterte Szenarien für XAML Islands in C++-Win32-Apps](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md)
* [Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App](host-custom-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien für XAML Islands in C++-Win32-Apps](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands-Codebeispiele](https://github.com/microsoft/Xaml-Islands-Samples)