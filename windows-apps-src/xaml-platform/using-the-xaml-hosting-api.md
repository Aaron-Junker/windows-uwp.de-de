---
description: Dieser Artikel beschreibt, wie Sie UWP-XAML-Benutzeroberfläche in Ihre desktop-Anwendung zu hosten.
title: Mithilfe der hosting-API in einer Desktopanwendung UWP XAML
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, Uwp, Windows Forms, Wpf, win32, XAML-Inseln
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9cb18abec43a4439b6d4750df797be5a1620a3aa
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63812754"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Mithilfe der hosting-API in einer Desktopanwendung UWP XAML

Ab Windows 10, Version 1903 sein, nicht-UWP-desktop-Anwendungen (einschließlich Windows Forms, WPF und C++ Win32-Anwendungen) können die *UWP XAML hosting-API* zum Hosten von UWP-Steuerelementen in einem UI-Element, die zugeordnet wird eine Das Fensterhandle (HWND). Mit dieser API können nicht-UWP-desktopanwendungen auf die neuesten Features von Windows 10-Benutzeroberfläche verwenden, die nur über die UWP-Steuerelemente verfügbar sind. Beispielsweise können nicht-UWP-desktop-Anwendungen verwenden diese API zum Hosten von UWP-Steuerelementen, mit denen die [Fluent-Entwurfssystem](../design/fluent-design-system/index.md) und unterstützen [Windows Ink](../design/input/pen-and-stylus-interactions.md).

Die UWP XAML hosting-API bildet die Grundlage für eine größere Gruppe von Steuerelementen, die wir bereit sind, um Entwicklern ermöglichen, Fluent-Benutzeroberfläche auf nicht-UWP-desktopanwendungen zu bringen. Dieses Feature heißt *XAML-Inseln*. Einen Überblick über diese Funktion finden Sie unter [UWP-Steuerelementen in desktopanwendungen](xaml-host-controls.md).

> [!NOTE]
> Wenn Sie Feedback zu XAML-Inseln haben, erstellen Sie ein neues Problem in der [Microsoft.Toolkit.Win32 Repository](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) und lassen Sie Ihre Kommentare vorhanden. Wenn Sie Ihr Feedback zu senden, privat möchten, können Sie senden, damit XamlIslandsFeedback@microsoft.com. Ihre Einblicke und Szenarien sind sehr wichtig für uns.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Sollten Sie die UWP XAML hosting-API verwenden?

Die UWP XAML hosting-API bietet die Low-Level-Infrastruktur zum Hosten von UWP-Steuerelementen in desktop-Anwendungen. Einige Arten von desktopanwendungen haben die Möglichkeit mit alternative und einfachere-APIs können Sie um dieses Ziel zu erreichen.  

* Wenn Sie eine C++-Win32-desktop-Anwendung verfügen, und Hosten von UWP-Steuerelementen in Ihrer Anwendung möchten, müssen Sie die UWP XAML hosting-API verwenden. Es gibt keine alternativen für diese Arten von Anwendungen.

* Für WPF und Windows Forms-Anwendungen, es wird dringend empfohlen, dass Sie verwenden die [umschlossen Steuerelemente](xaml-host-controls.md#wrapped-controls) und [Hoststeuerelemente](xaml-host-controls.md#host-controls) in das Windows-Community-Toolkit, anstatt die UWP XAML hosting-API direkt. Diese Steuerelemente verwenden die UWP XAML intern hosting-API und implementieren das Verhalten, die Sie andernfalls benötigen würden, um selbst zu behandeln, wenn Sie auf der hosting-API direkt, einschließlich Tastatur Navigation und das Layout Änderungen UWP XAML verwendet. Allerdings können Sie die UWP XAML hosting-API direkt in diese Arten von Anwendungen aus, falls gewünscht.

Dieser Artikel beschreibt, wie Sie mit der UWP XAML hosting-API direkt in der Anwendung anstelle der Steuerelemente, die von der Windows-Community-Toolkit bereitgestellt wird.

## <a name="prerequisites"></a>Vorraussetzungen

Die UWP XAML hosting-API verfügt über diese erforderlichen Komponenten:

* Windows 10, Version 1903 (oder höher) und den entsprechenden build des Windows SDK.
* Konfigurieren Sie das Projekt für die Verwendung von Windows-Runtime-APIs, und aktivieren Sie die XAML-Inseln anhand [diese Anweisungen](xaml-host-controls.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Architektur der XAML-Inseln

Die UWP XAML hosting-API enthält diese Haupttypen von Windows-Runtime und COM-Schnittstellen:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Diese Klasse stellt das UWP XAML-Framework. Diese Klasse stellt eine einzelne statische [ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) Methode, die UWP XAML-Framework für den aktuellen Thread in der Desktopanwendung initialisiert.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Diese Klasse stellt eine Instanz von UWP XAML-Inhalt, den Sie in Ihrer desktop-Anwendung hosten. Das wichtigste Element dieser Klasse ist die [ **Content** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) Eigenschaft. Sie weisen Sie diese Eigenschaft auf einen [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) , die Sie hosten möchten. Diese Klasse verfügt auch über andere Elemente für routing den Fokusnavigation per Tastatur in und aus der XAML-Inseln.

* **IDesktopWindowXamlSourceNative** und **IDesktopWindowXamlSourceNative2** COM-Schnittstellen. **IDesktopWindowXamlSourceNative** bietet die **AttachToWindow** -Methode, die Sie verwenden, um eine XAML-Dateninsel in der Anwendung für ein übergeordnetes Element der Benutzeroberfläche anzufügen. **IDesktopWindowXamlSourceNative2** bietet die **PreTranslateMessage** -Methode, die die UWP XAML-Framework bestimmte Windows-Nachrichten richtig verarbeiten kann.
    > [!NOTE]
    > Diese Schnittstellen deklariert werden, der **windows.ui.xaml.hosting.desktopwindowxamlsource.h** im Windows SDK-Headerdatei. Diese Datei wird standardmäßig in % ProgramFiles% (x86) %\Windows Kits\10\Include\\< Buildnummer\>\um. In einem C++-Win32-Projekt können Sie diese Header-Datei direkt verweisen. In einem WPF oder Windows Forms-Projekt müssen Sie deklarieren die Schnittstellen in Ihrer Anwendung Code mithilfe der [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) Attribut. Vergewissern Sie sich Ihre Schnittstellendeklarationen genau übereinstimmen, die Deklarationen in **windows.ui.xaml.hosting.desktopwindowxamlsource.h**.

Das folgende Diagramm veranschaulicht die Hierarchie der Objekte in einer XAML-Insel, die in einer desktop-Anwendung gehostet wird.

![DesktopWindowXamlSource architecture](images/xaml-hosting-api-rev2.png)

* Auf der Basisebene ist das Benutzeroberflächenelement in Ihrer Anwendung die, in dem der XAML-Insel gehostet werden soll. Dieses Benutzeroberflächenelement benötigen ein Fensterhandle (HWND). Beispiele für UI-Elemente, die in dem Sie eine XAML-Insel hosten [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) für WPF-Anwendungen [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) für Windows Forms-Anwendungen, und ein [Fenster](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) für C++ Win32-Anwendungen.

* Auf der nächsten Ebene ist ein **DesktopWindowXamlSource** Objekt. Dieses Objekt bietet die Infrastruktur zum Hosten der XAML-Insel an. Der Code ist verantwortlich für dieses Objekt erstellt und an das übergeordnete Element der Benutzeroberfläche anzufügen.

* Bei der Erstellung einer **DesktopWindowXamlSource**, dieses Objekt erstellt automatisch ein native untergeordnete Fenster zum Hosten Ihrer UWP-Steuerelements. Diese native untergeordnetes Fenster ist größtenteils von Ihrem Code abstrahiert können, aber Sie das Handle (HWND), falls erforderlich.

* Auf der obersten Ebene ist das UWP-Steuerelement, die, das Sie in Ihrer desktop-Anwendung hosten möchten. Dies kann jedes UWP-Objekt, das abgeleitet sein [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), einschließlich alle UWP-Steuerelement, die vom Windows SDK bereitgestellt werden sowie benutzerdefinierte Benutzersteuerelemente.

## <a name="related-samples"></a>Verwandte Beispiele

Wie Sie die UWP XAML hosting-API in Ihrem Code verwenden, hängt von Ihren Anwendungstyp, den Entwurf Ihrer Anwendung und anderen Faktoren ab. Um zu veranschaulichen, wie Sie diese API im Rahmen einer vollständigen Anwendung verwenden, bezieht sich in diesem Artikel auf Code aus den folgenden Beispielen.

### <a name="c-win32"></a>C++ Win32

[C++Beispiel für Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island). Dieses Beispiel veranschaulicht eine vollständige Implementierung des Hostings eines UWP-Benutzersteuerelements in ein entpackt C++ Win32-Anwendung (d. h. eine Anwendung, die nicht in ein MSIX-Paket erstellt wird).

### <a name="wpf-and-windows-forms"></a>WPF und Windows Forms

Die [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) in das Windows-Community-Toolkit-Steuerelement dient gewissermaßen ein Verweis-Beispiel für die UWP hosting-API in WPF und Windows Forms-Anwendungen verwenden. Der Quellcode ist verfügbar, an den folgenden Speicherorten:

  * Für die WPF-Version des Steuerelements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Abgeleitet von die WPF-Version [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Für die Windows Forms-Version des Steuerelements [finden Sie hier](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Abgeleitet von die Windows Forms-Version [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Die UWP XAML Host steuert.

Hier sind die wichtigsten Schritte für die Verwendung der UWP XAML hosting-API um eine UWP-Steuerelement in der Anwendung zu hosten.

1. Die UWP XAML-Framework für den aktuellen Thread initialisiert werden, bevor die Anwendung eines erstellt die [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) Objekte, die sie in hosten wird die [  **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Wenn Ihre Anwendung erstellt die **DesktopWindowXamlSource** Objekt vor dem Erstellen eines der **Windows.UI.Xaml.UIElement** Objekten, die dieses Framework wird für Sie initialisiert werden, beim Instanziieren der **DesktopWindowXamlSource** Objekt. In diesem Szenario müssen Sie Code zum Initialisieren des Frameworks selbst hinzufügen.

    * Jedoch, wenn Ihre Anwendung erstellt der **Windows.UI.Xaml.UIElement** Objekte vor dem Erstellen der **DesktopWindowXamlSource** -Objekt, das sie hostet der Anwendung muss die statische aufrufen[ **WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) Methode, um explizit die UWP XAML-Framework vor dem Initialisieren der **Windows.UI.Xaml.UIElement** Objekte sind instanziiert. Ihre Anwendung sollten diese Methode in der Regel aufrufen, wenn das übergeordnete Element der Benutzeroberfläche, hostet die **DesktopWindowXamlSource** instanziiert wird.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Diese Methode gibt eine [ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) -Objekt, das einen Verweis auf die UWP XAML-Framework enthält. Sie können so viele erstellen **WindowsXamlManager** Objekte wie für einen angegebenen Thread gewünscht. Aber da jedes Objekt einen Verweis auf die UWP XAML-Framework enthält, sollten Sie freigeben, die Objekte aus, um sicherzustellen, dass die XAML-Ressourcen schließlich freigegeben werden.

2. Erstellen Sie eine [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) Objekt aus, und fügen Sie ihn an einem übergeordneten UI-Element in Ihrer Anwendung, die einen Fenster-Handle zugeordnet ist.

    Zu diesem Zweck müssen Sie die folgenden Schritte ausführen:

    1. Erstellen Sie eine **DesktopWindowXamlSource** Objekt aus, und wandeln Sie sie in der **IDesktopWindowXamlSourceNative** oder **IDesktopWindowXamlSourceNative2** COM-Schnittstelle.

    2. Rufen Sie die **AttachToWindow** -Methode der der **IDesktopWindowXamlSourceNative** oder **IDesktopWindowXamlSourceNative2** -Schnittstelle ab, und übergeben Sie in der das Fensterhandle des der übergeordnete Element der Benutzeroberfläche in Ihrer Anwendung.

    3. Die Anfangsgröße des internen untergeordneten Fensters innerhalb der **DesktopWindowXamlSource**. Standardmäßig wird dieses interne untergeordnete Fenster auf eine Breite und Höhe von 0 festgelegt. Wenn Sie nicht die Größe des Fensters, alle UWP-Steuerelemente festlegen, Sie hinzufügen, die **DesktopWindowXamlSource** werden nicht angezeigt. Das interne untergeordnete Fenster in den Zugriff auf die **DesktopWindowXamlSource**, verwenden Sie die **WindowHandle** Eigenschaft der **IDesktopWindowXamlSourceNative** oder **IDesktopWindowXamlSourceNative2** Schnittstelle. Die folgenden Beispiele verwenden die [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) Funktion, um die Größe des Fensters festzulegen.

    Hier sind einige Codebeispiele, die diesen Prozess zu veranschaulichen.

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

3. Festlegen der **Windows.UI.Xaml.UIElement** zu gehostet werden soll die [ **Content** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) Eigenschaft Ihre **DesktopWindowXamlSource** Objekt. Im folgenden Beispiel wird eine [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) mit dem Namen ```myGrid``` auf die **Content** Eigenschaft.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Vollständige Beispiele, in denen diese Aufgaben im Rahmen eine funktionierende beispielanwendung veranschaulicht, finden Sie im folgenden Code:

  * **C++ Win32:** Finden Sie unter den [XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp) Datei die [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * **WPF:** Finden Sie unter den [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) Dateien in das Windows-Community-Toolkit.  

  * **Windows-Formulare:** Finden Sie unter den [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) und [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) Dateien in das Windows-Community-Toolkit.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Behandeln der Tastaturnavigation für Eingabe und Fokus

Es gibt mehrere Dinge, die Ihre app benötigt, um Eingabe und Fokus Tastaturnavigation ordnungsgemäß zu verarbeiten, wenn sie XAML-Inseln hostet.

### <a name="keyboard-input"></a>Tastatureingabe

Um Tastatureingaben für jedes XAML-Island richtig behandeln zu können, muss Ihre Anwendung alle Windows-Meldungen für die UWP XAML-Framework übergeben, damit bestimmte Nachrichten richtig verarbeitet werden können. Wandeln Sie hierzu an einer Stelle in Ihrer Anwendung, die die Nachrichtenschleife zugreifen können, die **DesktopWindowXamlSource** -Objekt für jedes XAML-Insel auf eine **IDesktopWindowXamlSourceNative2** COM-Schnittstelle. Rufen Sie dann die **PreTranslateMessage** -Methode der diese Schnittstelle und übergeben Sie die aktuelle Meldung.

  * Ein C++ Win32-Anwendung aufrufen kann **PreTranslateMessage** direkt in ihre primäre Meldungsschleife. Ein Beispiel finden Sie unter den [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61) Codedatei in die [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * Eine WPF-Anwendung aufrufen kann **PreTranslateMessage** aus der Ereignishandler für die [ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) Ereignis. Ein Beispiel finden Sie unter den [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) -Datei in das Windows-Community-Toolkit.

  * Eine Windows Forms-Anwendung aufrufen kann **PreTranslateMessage** aus eine Außerkraftsetzung für die [ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) Methode. Ein Beispiel finden Sie unter den [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) -Datei in das Windows-Community-Toolkit.

### <a name="keyboard-focus-navigation"></a>Den Tastaturfokus

Wenn der Benutzer navigiert, durch die Elemente der Benutzeroberfläche in Ihrer Anwendung mithilfe der Tastatur (z. B. durch Drücken von **Registerkarte** oder Richtung/oben-Taste), müssen Sie den Fokus programmgesteuert verschieben, in und aus der  **DesktopWindowXamlSource** Objekt. Wenn der Benutzer die Tastaturnavigation erreicht die **DesktopWindowXamlSource**, Verschieben des Fokus in die erste [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) Objekt in der Navigationsreihenfolge Fokus auf die folgenden für Ihre Benutzeroberfläche weiterhin **Windows.UI.Xaml.UIElement** Objekte, die der Benutzer durchläuft die Elemente, und verschieben den Fokus wieder der die **DesktopWindowXamlSource**und in das übergeordnete Element der Benutzeroberfläche.  

Die UWP XAML hosting-API enthält mehrere Typen und Member zum Umsetzen dieser Aufgaben beitragen.

* Wenn die Navigation per Tastatur eingibt Ihre **DesktopWindowXamlSource**, [ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) Ereignis wird ausgelöst. Dieses Ereignis behandeln und programmgesteuert verschieben des Fokus zur ersten gehosteten **Windows.UI.Xaml.UIElement** mithilfe der [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) Methode.

* Wenn der Benutzer ist, auf das letzte fokussierbare Element in Ihrer **DesktopWindowXamlSource** und drückt die **Registerkarte** Schlüssel oder eine Taste, die [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) Ereignis wird ausgelöst. Behandeln Sie dieses Ereignis aus, und verschieben Sie der Markierung programmgesteuert auf das nächste fokussierbare Element in der hostanwendung. Z. B. in einer WPF-Anwendung, in denen die **DesktopWindowXamlSource** gehostet wird, eine [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), können Sie die [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) Methode, um den Fokus auf das nächste fokussierbare Element in der hostanwendung zu übertragen.

Beispiele zur Vorgehensweise hierfür im Kontext des eine funktionierende beispielanwendung finden Sie die folgenden Codedateien:

  * **WPF:** Finden Sie unter den [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) -Datei in das Windows-Community-Toolkit.  

  * **Windows-Formulare:** Finden Sie unter den [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) -Datei in das Windows-Community-Toolkit.

## <a name="handle-layout-changes"></a>Behandeln Sie Änderungen am layout

Wenn der Benutzer die Größe des übergeordneten UI-Elements ändert, müssen Sie zum Verarbeiten von layoutänderungen erforderlichen, um sicherzustellen, die Ihre UWP-Steuerelemente, die zeigen, wie zu erwarten. Hier sind einige wichtigen Szenarien zu berücksichtigen.

* In einem C++ Win32-Anwendung, wenn Ihre Anwendung die WM_SIZE-Meldung können sie den gehosteten XAML-Insel neu positionieren verarbeitet, mit der [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) Funktion. Ein Beispiel finden Sie unter den [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) Codedatei in die [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Wenn das übergeordnete Element der Benutzeroberfläche muss zum Abrufen der Größe des rechteckigen Bereichs, der zum Anpassen der **Windows.UI.Xaml.UIElement** , die Sie hosten auf die **DesktopWindowXamlSource**, rufen Sie die [ **Measure** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) Methode der **Windows.UI.Xaml.UIElement**. Zum Beispiel:

    * In einer WPF-Anwendung möglicherweise erreichen Sie dies aus der [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) Methode der [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) , hostet die  **DesktopWindowXamlSource**. Ein Beispiel finden Sie unter den [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) -Datei in das Windows-Community-Toolkit.

    * In einer Windows Forms-Anwendung möglicherweise erreichen Sie dies aus der [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) Methode der [ **Steuerelement** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) , hostet die **DesktopWindowXamlSource**. Ein Beispiel finden Sie unter den [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) -Datei in das Windows-Community-Toolkit.

* Wenn die Größe des übergeordneten UI-Elements ändert, rufen Sie die [ **anordnen** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) Methode des Stamms **Windows.UI.Xaml.UIElement** , die Sie hosten auf die  **DesktopWindowXamlSource**. Zum Beispiel:

    * In einer WPF-Anwendung möglicherweise erreichen Sie dies aus der [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) Methode der [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) -Objekt, das hostet**DesktopWindowXamlSource**. Ein Beispiel finden Sie unter den [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) -Datei in das Windows-Community-Toolkit.

    * In Windows Forms-Anwendungen können diese Schritte werden der Handler für die [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) Ereignis die [ **Steuerelement** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) , hostet die **DesktopWindowXamlSource**. Ein Beispiel finden Sie unter den [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) -Datei in das Windows-Community-Toolkit.

## <a name="handle-dpi-changes"></a>Behandeln von DPI-Änderungen

Die UWP XAML-Framework kümmert sich DPI-Änderungen für die gehosteten UWP-Steuerelementen automatisch (z. B. wenn der Benutzer das Fenster zwischen Monitore mit anderen Bildschirm DPI zieht). Für optimale Ergebnisse, es wird empfohlen, die Ihrer Windows Forms, WPF oder C++ Win32-Anwendung ist so konfiguriert, dass monitorspezifische DPI-bewusst sein.

Um Ihrer Anwendung berücksichtigen DPI pro Monitor konfigurieren zu können, fügen einen [Seite-an-Seite-Assemblymanifest](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) auf Ihr Projekt und ```<dpiAwareness>``` Element ```PerMonitorV2```. Weitere Informationen zu diesen Wert, finden Sie in der Beschreibung für [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Host benutzerdefinierte UWP XAML-Steuerelemente

> [!IMPORTANT]
> Um ein benutzerdefiniertes UWP XAML-Steuerelement zu hosten, müssen Sie den Quellcode für das Steuerelement verfügen, damit Sie es in Ihrer Anwendung kompilieren können.

Wenn Sie ein benutzerdefiniertes UWP XAML-Steuerelement (eines Steuerelements, die Sie selbst definieren oder eine Kontrolle durch einen 3rd party) hosten möchten, müssen Sie die folgenden zusätzlichen Aufgaben zusätzlich zu den beschriebenen Prozess Ausführen der [vorherigen Abschnitt](#host-uwp-xaml-controls).

1. Definieren ein benutzerdefiniertes Typs, die von abgeleitet [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) und implementiert auch [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Dieser Typ fungiert als einen Stamm-Metadatenanbieter für Metadaten für benutzerdefinierte UWP XAML-Typen in Assemblys im aktuellen Verzeichnis der Anwendung geladen werden.

2. Rufen Sie die [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) -Methode der Ihre Metadaten Stammanbieter, wenn der Typname des UWP XAML-Steuerelements zugewiesen wird (Dies konnte im Code zur Laufzeit zugewiesen werden, oder Sie können entscheiden, diese zu aktivieren zugewiesen in Visual Studio-Eigenschaftenfenster).

    Beispiele finden Sie die folgenden Codedateien:
      * **C++ Win32:** Finden Sie unter den [XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp) Codedatei in die [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

      * **WPF und Windows Forms**: Finden Sie unter den [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) und [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) Codedateien in das Windows-Community-Toolkit. Diese Dateien sind Teil der Implementierung von freigegebenen der **WindowsXamlHost** Klassen für WPF und Windows-Formulare, wie Sie mit der UWP XAML-hosting-API in diese Arten von apps zu verdeutlichen.

3. Integrieren Sie den Quellcode für das benutzerdefinierte UWP XAML-Steuerelement in Ihrer Anwendungslösung die Hosts, erstellen Sie das benutzerdefinierte Steuerelement zu und verwenden Sie es in Ihrer Anwendung. Anleitungen für eine WPF oder Windows Forms-Anwendung finden Sie unter [diese Anweisungen](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control). Ein Beispiel für eine C++ Win32-Anwendung finden Sie unter den [Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup) und ["MyApp"](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp) -Projekte in der [ C++ Win32-Beispiel](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Fehler bei der Verwendung von UWP XAML hosting-API in einer UWP-app

| Problem | Auflösung |
|-------|------------|
| Ihre app empfängt eine **COMException** mit folgender Meldung: "DesktopWindowXamlSource kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-app verwendet werden." oder "WindowsXamlManager kann nicht aktiviert werden. Dieser Typ kann nicht in einer UWP-app verwendet werden." | Dieser Fehler zeigt an, Sie möchten die UWP XAML hosting-API zu verwenden (insbesondere möchten zum Instanziieren der [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) oder [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) Typen) in einer UWP-app. Die UWP XAML hosting-API ist nur in nicht-UWP-desktop-apps wie z. B. WPF, Windows Forms und C++-Win32-Anwendungen verwendet werden soll. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Fehler beim Anfügen an ein Fenster in einem anderen thread

| Problem | Auflösung |
|-------|------------|
| Ihre app empfängt eine **COMException** mit folgender Meldung: "AttachToWindow Methode Fehler auf, weil die angegebene HWND in einem anderen Thread erstellt wurde". | Dieser Fehler weist darauf hin, dass Ihre Anwendung wird aufgerufen, die **IDesktopWindowXamlSourceNative::AttachToWindow** Methode und übergeben sie das HWND des ein Fenster, das auf einem anderen Thread erstellt wurde. Sie müssen diese Methode das HWND des ein Fenster übergeben, die auf dem gleichen Thread wie der Code erstellt wurde, von dem Sie die Methode aufrufen. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Fehler beim Anhängen an ein Fenster auf einem anderen Fenster der obersten Ebene

| Problem | Auflösung |
|-------|------------|
| Ihre app empfängt eine **COMException** mit folgender Meldung: "AttachToWindow Methode Fehler auf, da es sich bei das angegebene HWND aus einem anderen auf oberster Ebene Fenster als das HWND abgeleitet, die zuvor auf dem gleichen Thread AttachToWindow übergeben wurde". | Dieser Fehler weist darauf hin, dass Ihre Anwendung wird aufgerufen, die **IDesktopWindowXamlSourceNative::AttachToWindow** Methode und übergeben sie das HWND des ein Fenster, das in einem anderen auf oberster Ebene Fenster als ein Fenster abgeleitet in angegebenen ein vorherigen Aufruf dieser Methode auf dem gleichen Thread.</p></p>Nach ruft Ihre Anwendung **IDesktopWindowXamlSourceNative::AttachToWindow** für einen bestimmten Thread, alle anderen [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) Objekte auf dem gleichen Threads kann nur auf Windows, die Nachfolger von demselben Fenster auf oberster Ebene, der dem ersten Aufruf übergeben wurde, sind Anfügen **IDesktopWindowXamlSourceNative::AttachToWindow**. Wenn alle der **DesktopWindowXamlSource** -Objekte geschlossen werden, für einen bestimmten Thread, der nächsten **DesktopWindowXamlSource** kann dann an ein beliebiges Fenster erneut anfügen.</p></p>Um dieses Problem zu beheben, schließen entweder alle **DesktopWindowXamlSource** Objekte, die, die an andere Fenster auf oberster Ebene für diesen Thread gebunden sind, oder erstellen einen neuen Thread für diese **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Verwandte Themen

* [UWP-Steuerelementen in desktop-Anwendungen](xaml-host-controls.md)
* [C++Beispiel für Win32-XAML-Inseln](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)
