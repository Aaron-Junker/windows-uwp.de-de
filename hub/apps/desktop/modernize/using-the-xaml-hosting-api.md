---
description: In diesem Artikel wird beschrieben, wie Sie UWP-XAML-Benutzeroberfläche in der Desktop Anwendung hosten.
title: Verwenden der UWP-XAML-Hosting-API in einer Desktop Anwendung
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, XAML-Inseln
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601535"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Verwenden der UWP-XAML-Hosting-API in einer Desktop Anwendung

Ab Windows 10, Version 1903, können nicht-UWP-Desktop Anwendungen (einschließlich WPF-, Windows Forms C++ -und Win32-Anwendungen) die UWP- *XAML-Hosting-API* verwenden, um UWP-Steuerelemente in jedem Benutzeroberflächen Element zu hosten, das einem Fenster Handle (HWND) zugeordnet ist. Mit dieser API können nicht-UWP-Desktop Anwendungen die neuesten Windows 10-Benutzeroberflächen Features verwenden, die nur über UWP-Steuerelemente zur Verfügung stehen. Beispielsweise können nicht-UWP-Desktop Anwendungen diese API verwenden, um UWP-Steuerelemente zu hosten, die das [fließende Design System](/windows/uwp/design/fluent-design-system/index) verwenden und [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)unterstützen.

Die UWP-XAML-Hosting-API stellt die Grundlage für einen umfassenderen Satz von Steuerelementen dar, die wir bereitstellen, damit Entwickler eine fließende Benutzeroberfläche für Desktop Anwendungen ohne UWP bereitstellen können. Diese Funktion wird als *XAML-Inseln*bezeichnet. Eine Übersicht über diese Funktion finden Sie unter [UWP-Steuerelemente in Desktop Anwendungen](xaml-islands.md).

> [!NOTE]
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem im Repository " [Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) ", und lassen Sie Ihre Kommentare dort ablegen. Wenn Sie Ihr Feedback lieber privat einreichen möchten, können Sie es an XamlIslandsFeedback@microsoft.comsenden. Ihre Einblicke und Szenarios sind für uns äußerst wichtig.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Sollten Sie die UWP-XAML-Hosting-API verwenden?

Die UWP-XAML-Hosting-API bietet die Low-Level-Infrastruktur zum Hosten von UWP-Steuerelementen in Desktop Anwendungen. Einige Arten von Desktop Anwendungen können alternative, bequemere APIs verwenden, um dieses Ziel zu erreichen.  

* Wenn Sie über eine C++ Win32-Desktop Anwendung verfügen und UWP-Steuerelemente in der Anwendung hosten möchten, müssen Sie die UWP-XAML-Hosting-API verwenden. Für diese Arten von Anwendungen gibt es keine Alternativen.

* Für WPF-und Windows Forms-Anwendungen wird dringend empfohlen, dass Sie die umschließenden Steuer [Elemente](xaml-islands.md#wrapped-controls) und [Host Steuerelemente](xaml-islands.md#host-controls) im Windows Community Toolkit verwenden, anstatt die UWP-XAML-Hosting-API direkt zu verwenden. Diese Steuerelemente verwenden intern die UWP-XAML-Hosting-API und implementieren das gesamte Verhalten, das Sie andernfalls selbst behandeln müssen, wenn Sie die UWP-XAML-Hosting-API direkt verwendet haben, einschließlich der Tastaturnavigation und der Layoutänderungen. Sie können die UWP-XAML-Hosting-API jedoch direkt in diesen Anwendungs Typen verwenden, wenn Sie auswählen.

In diesem Artikel wird beschrieben, wie Sie die UWP-XAML-Hosting-API direkt in Ihrer Anwendung anstelle der vom Windows Community Toolkit bereitgestellten Steuerelemente verwenden können.

## <a name="prerequisites"></a>Vorraussetzungen

Für die UWP-XAML-Hosting-API gelten die folgenden Voraussetzungen:

* Windows 10, Version 1903 (oder höher) und der entsprechende Build der Windows SDK.
* Konfigurieren Sie das Projekt für die Verwendung Windows-Runtime APIs, und aktivieren Sie XAML-Inseln, indem Sie [diese Anweisungen](xaml-islands.md#requirements)befolgen.

## <a name="architecture-of-xaml-islands"></a>Architektur der XAML-Inseln

Die UWP-XAML-Hosting-API umfasst diese Haupt Windows-Runtime Typen und COM-Schnittstellen:

* [**Windowsxamlmanager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Diese Klasse stellt das UWP-XAML-Framework dar. Diese Klasse stellt eine einzelne statische [**initializeforcurrentthread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) -Methode bereit, die das UWP-XAML-Framework auf dem aktuellen Thread in der Desktop Anwendung initialisiert.

* [**Desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Diese Klasse stellt eine Instanz des UWP-XAML-Inhalts dar, die Sie in der Desktop Anwendung gehostet haben. Der wichtigste Member dieser Klasse ist die [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) -Eigenschaft. Sie weisen diese Eigenschaft einem [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) zu, das Sie hosten möchten. Diese Klasse verfügt auch über andere Member zum Routing der Tastaturfokus Navigation in und aus den XAML-Inseln.

* **Idesktopwindowxamlsourcenative** und **IDesktopWindowXamlSourceNative2** com-Schnittstellen. **Idesktopwindowxamlsourcenative** stellt die **attachdewindow** -Methode bereit, mit der Sie eine XAML-Insel in der Anwendung an ein übergeordnetes Benutzeroberflächen Element anfügen können. **IDesktopWindowXamlSourceNative2** stellt die **pretranslatemess** -Methode bereit, die es dem UWP-XAML-Framework ermöglicht, bestimmte Windows-Meldungen ordnungsgemäß zu verarbeiten.
    > [!NOTE]
    > Diese Schnittstellen werden in der **Windows. UI. XAML. Hosting. desktopwindowxamlsource. h** -Header Datei im Windows SDK deklariert. Standardmäßig befindet sich diese Datei unter% Program Files (x86)% \ Windows kits\10\include\\< Buildnummer\>\in. In einem C++ Win32-Projekt können Sie direkt auf diese Header Datei verweisen. In einem WPF-oder Windows Forms-Projekt müssen Sie die Schnittstellen im Anwendungscode mithilfe des [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) -Attributs deklarieren. Stellen Sie sicher, dass ihre Schnittstellen Deklarationen exakt mit den Deklarationen in **Windows. UI. XAML. Hosting. desktopwindowxamlsource. h**übereinstimmen.

Das folgende Diagramm veranschaulicht die Hierarchie von Objekten in einer XAML-Insel, die in einer Desktop Anwendung gehostet wird.

![Desktopwindowxamlsource-Architektur](images/xaml-islands/xaml-hosting-api-rev2.png)

* Auf der Basisebene ist das Benutzeroberflächen Element in der Anwendung, in dem Sie die XAML-Insel hosten möchten. Dieses UI-Element muss über ein Fenster Handle (HWND) verfügen. Beispiele für Elemente der Benutzeroberfläche, in denen Sie eine XAML-Insel hosten können, sind [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) für WPF-Anwendungen, [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) für C++ Windows Forms Anwendungen und ein [Fenster](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) für Win32 Bereich.

* Auf der nächsten Ebene befindet sich ein **desktopwindowxamlsource** -Objekt. Dieses Objekt stellt die Infrastruktur zum Hosting der XAML-Insel bereit. Der Code ist dafür verantwortlich, dieses Objekt zu erstellen und an das übergeordnete UI-Element anzufügen.

* Wenn Sie eine **desktopwindowxamlsource**erstellen, erstellt dieses Objekt automatisch ein natives untergeordnetes Fenster, um das UWP-Steuerelement zu hosten. Dieses systemeigene untergeordnete Fenster wird größtenteils aus dem Code abstrahiert, aber Sie können ggf. auf das Handle (HWND) zugreifen.

* Auf der obersten Ebene befindet sich schließlich das UWP-Steuerelement, das Sie in Ihrer Desktop Anwendung hosten möchten. Dabei kann es sich um ein beliebiges UWP-Objekt handeln, das von " [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)" abgeleitet wird, einschließlich sämtlicher UWP-Steuerelemente, die von der Windows SDK bereitgestellt werden, sowie

## <a name="related-samples"></a>Verwandte Beispiele

Die Art und Weise, in der Sie die UWP-XAML-Hosting-API in Ihrem Code verwenden, hängt von Ihrem Anwendungstyp, dem Design Ihrer Anwendung und anderen Faktoren ab. Um zu veranschaulichen, wie diese API im Kontext einer kompletten Anwendung verwendet wird, bezieht sich dieser Artikel auf Code aus den folgenden Beispielen.

### <a name="c-win32"></a>C++Win32

Win32-Beispiel. [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App) Dieses Beispiel veranschaulicht eine komplette Implementierung des Hostings eines UWP-Benutzer Steuer Elements in C++ einer nicht verpackten Win32-Anwendung (d. h. eine Anwendung, die nicht in ein msix-Paket integriert ist).

### <a name="wpf-and-windows-forms"></a>WPF und Windows Forms

Das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement im Windows Community Toolkit fungiert als Referenzbeispiel für die Verwendung der UWP-Hosting-API in WPF-und Windows Forms-Anwendungen. Der Quellcode ist an den folgenden Speicherorten verfügbar:

  * Die WPF-Version des Steuer Elements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Die WPF-Version wird von [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)abgeleitet.

  * Die Windows Forms-Version des Steuer Elements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Die Windows Forms Version wird von [**System. Windows. Forms. Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)abgeleitet.

## <a name="host-uwp-xaml-controls"></a>UWP-XAML-Steuerelemente für Host

Im folgenden finden Sie die wichtigsten Schritte zum Hosten eines UWP-Steuer Elements in Ihrer Anwendung mithilfe der UWP-XAML-Hosting-API.

1. Initialisieren Sie das UWP-XAML-Framework für den aktuellen Thread, bevor die Anwendung eines der [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) -Objekte erstellt, die in der [**desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)gehostet werden.

    * Wenn Ihre Anwendung das **desktopwindowxamlsource** -Objekt erstellt, bevor Sie eines der **Windows. UI. XAML. UIElement** -Objekte erstellt haben, wird dieses Framework für Sie initialisiert, wenn Sie das **desktopwindowxamlsource** -Objekt instanziieren. . In diesem Szenario müssen Sie keinen eigenen Code hinzufügen, um das Framework zu initialisieren.

    * Wenn die Anwendung jedoch die **Windows. UI. XAML. UIElement** -Objekte erstellt, bevor das **desktopwindowxamlsource** -Objekt erstellt wird, das Sie hostet, muss Ihre Anwendung die statische [ **Windowsxamlmanager. initializeforcurrentthread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) -Methode, um das UWP-XAML-Framework explizit zu initialisieren, bevor die **Windows. UI. XAML. UIElement** -Objekte instanziiert werden. Diese Methode sollte in der Regel von Ihrer Anwendung aufgerufen werden, wenn das übergeordnete UI-Element, das **desktopwindowxamlsource** hostet, instanziiert wird.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Diese Methode gibt ein [**windowsxamlmanager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) -Objekt zurück, das einen Verweis auf das UWP-XAML-Framework enthält. Sie können in einem bestimmten Thread beliebig viele **windowsxamlmanager** -Objekte erstellen. Da jedes Objekt jedoch einen Verweis auf das UWP-XAML-Framework enthält, sollten Sie die Objekte verwerfen, um sicherzustellen, dass XAML-Ressourcen schließlich freigegeben werden.

2. Erstellen Sie ein [**desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) -Objekt, und fügen Sie es an ein übergeordnetes Benutzeroberflächen Element in der Anwendung an, das einem Fenster Handle zugeordnet ist.

    Zu diesem Zweck müssen Sie die folgenden Schritte ausführen:

    1. Erstellen Sie ein **desktopwindowxamlsource** -Objekt, und wandeln Sie es in die Schnittstelle **idesktopwindowxamlsourcenative** oder **IDesktopWindowXamlSourceNative2** com um.

    2. Ruft die **attachtowindow** -Methode der **idesktopwindowxamlsourcenative** -Schnittstelle oder der **IDesktopWindowXamlSourceNative2** -Schnittstelle auf und übergibt das Fenster Handle des übergeordneten UI-Elements in der Anwendung.

    3. Legen Sie die anfängliche Größe des internen untergeordneten Fensters fest, das in der **desktopwindowxamlsource**enthalten ist. Standardmäßig ist dieses interne untergeordnete Fenster auf eine Breite und Höhe von 0 festgelegt. Wenn Sie die Größe des Fensters nicht festlegen, werden alle UWP-Steuerelemente, die Sie der **desktopwindowxamlsource** hinzufügen, nicht angezeigt. Um auf das interne untergeordnete Fenster in der **desktopwindowxamlsource**zuzugreifen, verwenden Sie die **WindowHandle** -Eigenschaft der **idesktopwindowxamlsourcenative** -Schnittstelle oder der **IDesktopWindowXamlSourceNative2** -Schnittstelle. In den folgenden Beispielen wird die [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) -Funktion verwendet, um die Größe des Fensters festzulegen.

    Im folgenden finden Sie einige Codebeispiele, die diesen Prozess veranschaulichen.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. Legen Sie die **Windows. UI. XAML. UIElement** -Eigenschaft, die Sie hosten möchten, auf die [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) -Eigenschaft des **desktopwindowxamlsource** -Objekts fest. Im folgenden Beispiel wird ein [**Windows. UI. XAML. Controls. Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) - ```myGrid``` Element mit dem Namen auf die **Content** -Eigenschaft festgelegt.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Ausführliche Beispiele, die diese Aufgaben im Kontext einer funktionierenden Beispielanwendung veranschaulichen, finden Sie in den folgenden Code Dateien:

  * **C++Win32** Weitere Informationen finden Sie in der Datei " [xamlbridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) " im [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF** Weitere Informationen finden Sie in den Dateien [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) im Windows Community Toolkit.  

  * **Windows Forms:** Weitere Informationen finden Sie in den Dateien [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) im Windows Community Toolkit.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Behandeln von Tastatureingaben und Fokus Navigation

Es gibt mehrere Dinge, die Ihre APP tun muss, um die Tastatureingabe und die Fokus Navigation beim Hosten von XAML-Inseln ordnungsgemäß zu verarbeiten.

### <a name="keyboard-input"></a>Tastatureingabe

Damit Tastatureingaben für jede XAML-Insel ordnungsgemäß behandelt werden, muss die Anwendung alle Windows-Meldungen an das UWP-XAML-Framework übergeben, damit bestimmte Nachrichten ordnungsgemäß verarbeitet werden können. Wandeln Sie das **desktopwindowxamlsource** -Objekt für jede XAML-Insel in eine **IDesktopWindowXamlSourceNative2** -com-Schnittstelle ein, um dies zu erreichen. Rufen Sie dann die **pretranslatemess Age** -Methode dieser Schnittstelle auf, und übergeben Sie die aktuelle Nachricht.

  * In C++ einer Win32-Anwendung kann **pretranslatemess** direkt in der Hauptnachrichten Schleife aufgerufen werden. Ein Beispiel finden Sie in der Datei " [xamlbridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) " im [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * Eine WPF-Anwendung kann **pretranslatemess** aus dem Ereignishandler für das [**ComponentDispatcher. ThreadFilterMessage**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) -Ereignis aufrufen. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) im Windows Community Toolkit.

  * Eine Windows Forms Anwendung kann **pretranslatemess** aus einer außer Kraft setzung für die [**Control. PreProcessMessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) -Methode abrufen. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) im Windows Community Toolkit.

### <a name="keyboard-focus-navigation"></a>Tastaturfokus Navigation

Wenn der Benutzer die Benutzeroberflächen Elemente in der Anwendung über die Tastatur navigiert (z. b. durch Drücken der **Tab** -Taste oder der Richtung/Pfeiltaste), müssen Sie den Fokus Programm gesteuert auf das **desktopwindowxamlsource** -Objekt verschieben. Wenn die Tastaturnavigation des Benutzers die **desktopwindowxamlsource**erreicht, verschieben Sie den Fokus auf das erste [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) -Objekt in der Navigations Reihenfolge für Ihre Benutzeroberfläche, und verschieben Sie den Fokus weiter auf Folgendes **: Windows. UI. XAML. UIElement** -Objekte, während der Benutzer die Elemente durchläuft, und verschieben Sie dann den Fokus aus **desktopwindowxamlsource** und in das übergeordnete UI-Element zurück.  

Die UWP-XAML-Hosting-API bietet verschiedene Typen und Member, die Sie beim Ausführen dieser Aufgaben unterstützen.

* Wenn die Tastaturnavigation in Ihre **desktopwindowxamlsource**gelangt, wird das Ereignis " [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) " ausgelöst. Behandeln Sie dieses Ereignis, und verschieben Sie den Fokus Programm gesteuert auf das erste gehostete **Windows. UI. XAML. UIElement** mithilfe der [**navigatefocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) -Methode.

* Wenn der Benutzer das letzte Fokussier Bare Element in der **desktopwindowxamlsource** verwendet und die **Tab** -Taste oder eine Pfeiltaste drückt, wird das Ereignis " [**takefoceangeforderten**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) " ausgelöst. Behandeln Sie dieses Ereignis, und verschieben Sie den Fokus Programm gesteuert auf das nächste Fokussier Bare Element in der Host Anwendung. Beispielsweise können Sie in einer WPF-Anwendung, in der **DesktopWindowXamlSource** in einem [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) gehostet wird, die Methode [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) verwenden, um den Fokus auf das nächste fokussierbare Element in der Hostanwendung zu übertragen.

Beispiele für die Vorgehensweise im Kontext einer funktionierenden Beispielanwendung finden Sie in den folgenden Code Dateien:

  * **WPF** Weitere Informationen finden Sie in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) im Windows Community Toolkit.  

  * **Windows Forms:** Weitere Informationen finden Sie in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) im Windows Community Toolkit.

  * **C++/Win32**: Weitere Informationen finden Sie in der Datei " [xamlbridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) " im [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

## <a name="handle-layout-changes"></a>Behandeln von Layoutänderungen

Wenn der Benutzer die Größe des übergeordneten Elements der Benutzeroberfläche ändert, müssen Sie alle notwendigen Layoutänderungen verarbeiten, um sicherzustellen, dass Ihre UWP-Steuerelemente erwartungsgemäß angezeigt werden. Hier sind einige wichtige Szenarien, die berücksichtigt werden müssen.

* Wenn die C++ Anwendung in einer Win32-Anwendung die WM_SIZE-Nachricht verarbeitet, kann Sie die gehostete XAML-Insel mithilfe der [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) -Funktion neu positionieren. Ein Beispiel finden Sie in der Codedatei " [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) " im [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Wenn das übergeordnete Element der Benutzeroberfläche die Größe des rechteckigen Bereichs abrufen muss, der für das **Windows. UI. XAML. UIElement** erforderlich ist, das Sie auf dem **desktopwindowxamlsource**-Element gehostet haben, müssen Sie die [**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) -Methode von **Windows. UI. XAML. UIElement aufzurufen.** . Zum Beispiel:

    * In einer WPF-Anwendung können Sie dies über die " [**accessreoverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) "-Methode von [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) tun, der " **desktopwindowxamlsource**" hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

    * In einer Windows Forms Anwendung können Sie dies über die [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) -Methode des [**Steuer**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Elements tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

* Wenn sich die Größe des übergeordneten Elements der Benutzeroberfläche ändert, können Sie die [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)-Methode des Stammelements **Windows.UI.Xaml.UIElement** aufrufen, das Sie auf der **DesktopWindowXamlSource** hosten. Zum Beispiel:

    * In einer WPF-Anwendung können Sie dies über die [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) -Methode des [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) -Objekts tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

    * In einer Windows Forms Anwendung können Sie dies über den Handler für das [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) -Ereignis des [**Steuer**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Elements tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

## <a name="handle-dpi-changes"></a>Behandeln von dpi-Änderungen

Das UWP-XAML-Framework verarbeitet dpi-Änderungen für gehostete UWP-Steuerelemente automatisch (z. b. wenn der Benutzer das Fenster zwischen Monitoren mit unterschiedlichen Bildschirm dpi zieht). Um die optimale Leistung zu erzielen, empfiehlt es sich, Ihre Windows Forms-, C++ WPF-oder Win32-Anwendung so zu konfigurieren, dass Sie pro Monitor-dpi-Wert ist.

Fügen Sie Ihrem Projekt ein paralleles Assemblymanifest hinzu, und legen Sie das [-](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) Element auf ```<dpiAwareness>``` ```PerMonitorV2```fest, um die Anwendung für die dpi-Unterstützung pro Monitor zu konfigurieren. Weitere Informationen zu diesem Wert finden Sie in der Beschreibung für [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Host benutzerdefinierte UWP-XAML-Steuerelemente

Führen Sie diese allgemeinen Schritte aus, um ein benutzerdefiniertes UWP-XAML-Steuerelement zu hosten (entweder ein von Ihnen definiertes Steuerelement oder ein von einem Drittanbieter bereitgestelltes Steuerelement). Sie müssen über den Quellcode für das benutzerdefinierte Steuerelement verfügen, damit Sie es mit Ihrer Anwendung kompilieren können.

1. Integrieren Sie in Ihrer Host Anwendungslösung den Quellcode für das benutzerdefinierte UWP-XAML-Steuerelement, und erstellen Sie das benutzerdefinierte Steuerelement.

2. Fügen Sie der Host Anwendungslösung ein UWP-Anwendungsprojekt hinzu, und fügen Sie diesem Projekt das nuget-Paket [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) hinzu.

3. Ändern Sie im UWP-Anwendungsprojekt die `App` Klasse so, dass Sie aus der **Microsoft. Toolkit. Win32. UI. xamlapplication** -Klasse, die durch das nuget-Paket [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) verfügbar gemacht wird. Dieser Typ fungiert als Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen in Assemblys im aktuellen Verzeichnis der Anwendung.

4. Nennen Sie `App` im Konstruktor der Klasse die **Initialize** -Methode der Basisklasse **Microsoft. Toolkit. Win32. UI. xamlapplication** .

5. Befolgen Sie im Host Anwendungsprojekt das im [vorherigen Abschnitt](#host-uwp-xaml-controls) beschriebene Verfahren, um das benutzerdefinierte Steuerelement in einer XAML-Insel zu hosten.

Ein Beispiel für eine C++ Win32-Anwendung finden Sie in den folgenden Projekten im [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): Dieses Projekt implementiert ein benutzerdefiniertes UWP-XAML-Steuerelement mit dem Namen `MyUserControl` , das ein Textfeld, mehrere Schaltflächen und ein Kombinations Feld enthält.
* [Myapp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): Dies ist ein UWP-Anwendungsprojekt mit den Änderungen, die in den vorherigen Schritten beschrieben wurden.
* [Samplecppapp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): Dies ist das C++ Win32-App-Projekt, das das benutzerdefinierte UWP-XAML-Steuerelement in einer XAML-Insel hostet.

Ausführliche Anweisungen und Beispiele für eine WPF-oder Windows Forms-Anwendung finden Sie in [diesen Anweisungen](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Fehler beim Verwenden der UWP-XAML-Hosting-API in einer UWP-App

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "Desktopwindowxamlsource kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden. " oder "windowsxamlmanager kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-App verwendet werden. " | Dieser Fehler weist darauf hin, dass Sie versuchen, die UWP-XAML-Hosting-API (insbesondere die Typen " [**desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) " oder " [**windowsxamlmanager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ") in einer UWP-APP zu verwenden. Die UWP-XAML-Hosting-API ist nur für die Verwendung in Desktop-Apps ohne UWP (z. b. WPF C++ -, Windows Forms-und Win32-Anwendungen) vorgesehen. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Fehler beim Anfügen an ein Fenster in einem anderen Thread.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "Fehler bei der attachywindow-Methode, weil das angegebene HWND in einem anderen Thread erstellt wurde". | Dieser Fehler weist darauf hin, dass die Anwendung die **idesktopwindowxamlsourcenative:: attachdewindow** -Methode aufgerufen hat und ihr das HWND eines Fensters, das in einem anderen Thread erstellt wurde, weitergereicht hat. Sie müssen diese Methode als HWND eines Fensters übergeben, das im gleichen Thread wie der Code erstellt wurde, von dem aus Sie die-Methode aufrufen. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Fehler beim Anfügen an ein Fenster in einem anderen Fenster der obersten Ebene.

| Problem | Auflösung |
|-------|------------|
| Ihre APP empfängt eine **COMException** mit der folgenden Meldung: "Die attachtewindow-Methode ist fehlgeschlagen, weil das angegebene HWND von einem anderen Fenster der obersten Ebene abweicht als das HWND, das zuvor an attachtewindow im gleichen Thread weitergegeben wurde." | Dieser Fehler zeigt an, dass die Anwendung die **idesktopwindowxamlsourcenative:: attachdewindow** -Methode aufgerufen hat und ihr das HWND eines Fensters, das von einem anderen Fenster der obersten Ebene abweicht, als ein Fenster, das Sie in einem vorherigen Aufruf dieser Methode angegeben haben, an Sie übermittelt hat. im gleichen Thread.</p></p>Nachdem Ihre Anwendung **idesktopwindowxamlsourcenative:: attachdewindow** für einen bestimmten Thread aufgerufen hat, können alle anderen [**desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) -Objekte im gleichen Thread nur an Fenster angefügt werden, die Nachfolger derselben obersten Ebene sind. Das Fenster, das beim ersten Aufruf von **idesktopwindowxamlsourcenative:: AttachTo Window**übermittelt wurde. Wenn alle **desktopwindowxamlsource** -Objekte für einen bestimmten Thread geschlossen sind, kann die nächste **desktopwindowxamlsource** wieder an jedes Fenster angefügt werden.</p></p>Um dieses Problem zu beheben, schließen Sie entweder alle **desktopwindowxamlsource** -Objekte, die an andere Fenster der obersten Ebene in diesem Thread gebunden sind, oder erstellen Sie einen neuen Thread für diese **desktopwindowxamlsource**. |

## <a name="related-topics"></a>Verwandte Themen

* [UWP-Steuerelemente in Desktop Anwendungen](xaml-islands.md)
* [C++Beispiel für Win32-XAML-Inseln](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
