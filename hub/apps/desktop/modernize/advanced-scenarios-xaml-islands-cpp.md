---
description: In diesem Artikel werden erweiterte Szenarien für die XAML C++ -Insel Hosting für Win32-apps erläutert.
title: Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, cpp, Win32, XAML-Inseln, umschließende Steuerelemente, Standard Steuerelemente
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226234"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Erweiterte Szenarien für XAML-Inseln C++ in Win32-apps

Das [Host a Standard-UWP-Steuer](host-standard-control-with-xaml-islands-cpp.md) Element und das [Hosten einer benutzerdefinierten UWP-Steuerungs](host-custom-control-with-xaml-islands-cpp.md) Artikel enthalten Anweisungen und Beispiele zum C++ Hosten von XAML-Inseln in einer Win32-App In den Codebeispielen in diesen Artikeln werden jedoch nicht viele erweiterte Szenarien behandelt, die Desktop Anwendungen möglicherweise verarbeiten müssen, um eine reibungslose Benutzerfunktion bereitzustellen. Dieser Artikel enthält Anleitungen für einige dieser Szenarien und Zeiger auf Verwandte Codebeispiele.

## <a name="keyboard-input"></a>Tastatureingabe

Damit Tastatureingaben für jede XAML-Insel ordnungsgemäß behandelt werden, muss die Anwendung alle Windows-Meldungen an das UWP-XAML-Framework übergeben, damit bestimmte Nachrichten ordnungsgemäß verarbeitet werden können. Wandeln Sie das **desktopwindowxamlsource** -Objekt für jede XAML-Insel in eine **IDesktopWindowXamlSourceNative2** -com-Schnittstelle ein, um dies zu erreichen. Rufen Sie dann die **pretranslatemess Age** -Methode dieser Schnittstelle auf, und übergeben Sie die aktuelle Nachricht.

  * Win32:: die APP kann **pretranslatemess** direkt in der Hauptnachrichten Schleife aufzurufen. **C++** Ein Beispiel finden Sie in der Datei " [xamlbridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) ".

  * **WPF:** Die APP kann **pretranslatemess** aus dem Ereignishandler für das [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) -Ereignis aufrufen. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) im Windows Community Toolkit.

  * **Windows Forms:** Die APP kann **pretranslatemess** aus einer außer Kraft setzung für die [Control. PreProcessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) -Methode abrufen. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) im Windows Community Toolkit.

## <a name="keyboard-focus-navigation"></a>Tastaturfokus Navigation

Wenn der Benutzer die Benutzeroberflächen Elemente in der Anwendung über die Tastatur navigiert (z. b. durch Drücken der **Tab** -Taste oder der Richtung/Pfeiltaste), müssen Sie den Fokus Programm gesteuert auf das **desktopwindowxamlsource** -Objekt verschieben. Wenn die Tastaturnavigation des Benutzers die **desktopwindowxamlsource**erreicht, verschieben Sie den Fokus auf das erste [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) -Objekt in der Navigations Reihenfolge für die Benutzeroberfläche, verschieben Sie den Fokus auf die folgenden **Windows. UI. XAML. UIElement** -Objekte, während der Benutzer die Elemente durchläuft, und verschieben Sie dann den Fokus aus **desktopwindowxamlsource** und in das übergeordnete UI  

Die UWP-XAML-Hosting-API bietet verschiedene Typen und Member, die Sie beim Ausführen dieser Aufgaben unterstützen.

* Wenn die Tastaturnavigation in Ihre **desktopwindowxamlsource**gelangt, wird das Ereignis " [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) " ausgelöst. Behandeln Sie dieses Ereignis, und verschieben Sie den Fokus Programm gesteuert auf das erste gehostete **Windows. UI. XAML. UIElement** mithilfe der [navigatefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) -Methode.

* Wenn der Benutzer das letzte Fokussier Bare Element in der **desktopwindowxamlsource** verwendet und die **Tab** -Taste oder eine Pfeiltaste drückt, wird das Ereignis " [takefoceangeforderten](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) " ausgelöst. Behandeln Sie dieses Ereignis, und verschieben Sie den Fokus Programm gesteuert auf das nächste Fokussier Bare Element in der Host Anwendung. Beispielsweise können Sie in einer WPF-Anwendung, in der **desktopwindowxamlsource** in einem [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)gehostet wird, die Methode " [wvefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) " verwenden, um den Fokus auf das nächste Fokussier Bare Element in der Host Anwendung zu übertragen.

Beispiele für die Vorgehensweise im Kontext einer funktionierenden Beispielanwendung finden Sie in den folgenden Code Dateien:

  * /Win32: Weitere Informationen finden Sie in der Datei " [xamlbridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) ".  **C++**

  * **WPF:** Weitere Informationen finden Sie in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) im Windows Community Toolkit.  

  * **Windows Forms:** Weitere Informationen finden Sie in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) im Windows Community Toolkit.

## <a name="handle-layout-changes"></a>Behandeln von Layoutänderungen

Wenn der Benutzer die Größe des übergeordneten Elements der Benutzeroberfläche ändert, müssen Sie alle notwendigen Layoutänderungen verarbeiten, um sicherzustellen, dass Ihre UWP-Steuerelemente erwartungsgemäß angezeigt werden. Hier sind einige wichtige Szenarien, die berücksichtigt werden müssen.

* Wenn die C++ Anwendung in einer Win32-Anwendung die WM_SIZE Nachricht verarbeitet, kann Sie die gehostete XAML-Insel mithilfe der [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) -Funktion neu positionieren. Ein Beispiel finden Sie in der Codedatei [SampleApp. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) .

* Wenn das übergeordnete Element der Benutzeroberfläche die Größe des rechteckigen Bereichs abrufen muss, der für das **Windows. UI. XAML. UIElement** erforderlich ist, das Sie auf dem **desktopwindowxamlsource**-Element gehostet haben, müssen Sie die [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) -Methode von **Windows. UI. XAML. UIElement**aufzurufen. Beispiel:

    * In einer WPF-Anwendung können Sie dies über die " [accessreoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) "-Methode von [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) tun, der " **desktopwindowxamlsource**" hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

    * In einer Windows Forms Anwendung können Sie dies über die [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) -Methode des [Steuer](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Elements tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

* Wenn sich die Größe des übergeordneten Elements der Benutzeroberfläche ändert, können Sie die [Anordnungs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) Methode des Stamm- **Windows. UI. XAML. UIElement-Elements** , das Sie auf der **desktopwindowxamlsource**-Klasse Hosting, abrufen. Beispiel:

    * In einer WPF-Anwendung können Sie dies über die [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) -Methode des [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) -Objekts tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

    * In einer Windows Forms Anwendung können Sie dies über den Handler für das [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) -Ereignis des [Steuer](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Elements tun, das **desktopwindowxamlsource**hostet. Ein Beispiel finden Sie in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows Community Toolkit.

## <a name="handle-dpi-changes"></a>Behandeln von dpi-Änderungen

Das UWP-XAML-Framework verarbeitet dpi-Änderungen für gehostete UWP-Steuerelemente automatisch (z. b. wenn der Benutzer das Fenster zwischen Monitoren mit unterschiedlichen Bildschirm dpi zieht). Um die optimale Leistung zu erzielen, empfiehlt es sich, Ihre Windows Forms-, C++ WPF-oder Win32-Anwendung so zu konfigurieren, dass Sie pro Monitor-dpi-Wert ist.

Fügen Sie Ihrem Projekt ein paralleles Assemblymanifest hinzu, und [side-by-side assembly manifest](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) legen Sie das **\<dpiawareness\>** -Element auf **PerMonitorV2**fest, um die Anwendung für die dpi-Erkennung pro Monitor zu konfigurieren. Weitere Informationen zu diesem Wert finden Sie in der Beschreibung für [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="related-topics"></a>Verwandte Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML-Inseln)](xaml-islands.md)
* [Verwenden der UWP-XAML-Hosting- C++ API in einer Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines UWP-Standard Steuer C++ Elements in einer Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten UWP- C++ Steuer Elements in einer Win32-App](host-custom-control-with-xaml-islands-cpp.md)
* [Codebeispiele für XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Beispiel für Win32-XAML-Inseln](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
